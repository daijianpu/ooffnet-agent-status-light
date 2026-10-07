# Adapter Integration

This document describes the small public contract for an AI-agent adapter. It
is intentionally not an SDK and does not publish the OOFF NET production
implementation.

## Architecture

```text
AI agent lifecycle
        ↓
Your adapter / hook
        ↓  one local JSON state message
OOFF NET local integration endpoint
        ↓
OOFF NET application
        ↓
USB-connected physical status light
```

The adapter observes the agent's lifecycle and reports an Agent state. It must
not forward prompts, model responses, source code, tool arguments, tool
outputs, credentials, or file contents.

## Adapter identity

An adapter is identified by a stable ID. The public manifest shape is:

```json
{
  "schema": 1,
  "id": "example.agent",
  "name": "Example Agent",
  "version": "1.0.0",
  "platforms": ["windows"]
}
```

The `id` is a routing key and must be stable, lowercase, and unique for the
adapter. It is not caller authentication. The adapter ID must be registered by
the local OOFF NET application before state messages are accepted.

## State message

On Windows, the local endpoint is the named pipe:

```text
\\.\pipe\OOFFNETAgentTrafficLight
```

Send one UTF-8 JSON object per line. The smallest valid Agent-state message is:

```json
{"type":"agent_state","adapter_id":"example.agent","state":"WORKING"}
```

`state` must be one of `IDLE`, `WORKING`, `WAITING`, `COMPLETED`, `ERROR`, or
`RECOVERING`. The message must use the adapter ID registered with OOFF NET.

## Optional diagnostic metadata

An adapter may include short diagnostic metadata when its runtime provides it:

```json
{
  "type": "agent_state",
  "adapter_id": "example.agent",
  "state": "WORKING",
  "source": "example_hook",
  "session_id": "session-123",
  "turn_id": "turn-7",
  "hook_event_name": "turn_started"
}
```

These fields help diagnose event ordering and duplicate delivery. They do not
add states, change the device protocol, or authorize access. Do not put prompt
text or other sensitive content in them.

## Response and failure behavior

The endpoint returns a JSON response for a request. A successful state delivery
is indicated by `ok: true` or, when the state is queued for the device runtime,
`queued: true`. An error response contains `ok: false` and an error name.

An adapter should treat a failed delivery as a local integration problem and
should fail open: it must not block the user's agent, prompt submission, tool
execution, or normal completion just because the light is unavailable.

## Binding and discovery

The OOFF NET application owns adapter discovery and device binding. Discovery
does not prove that an agent hook is installed or active. A user must bind the
adapter to a physical light in the application before state changes can be
displayed.

The application may expose a catalog and a binding operation, but adapter
authors should not modify device registry files or the physical-device
protocol directly.

## Lifecycle mapping

Map the events of your agent to the public meanings in
[`STATUS-SIGNALS.md`](STATUS-SIGNALS.md). A typical mapping is:

```text
turn accepted / tool work begins   → WORKING
permission or user input needed    → WAITING
turn completed                     → COMPLETED
recovering after a transient error → RECOVERING
unrecoverable failure              → ERROR
session inactive                   → IDLE
```

Do not emit a guessed state merely because a desktop process exists. Do not
replace a real lifecycle event with a timer when the agent provides a better
signal.

## Security and privacy boundary

The endpoint is local to the user's machine and is protected by the OOFF NET
application. The adapter is still responsible for limiting its own payloads to
the public contract. Never log or transmit secrets, prompts, responses, source
code, or file contents as part of adapter diagnostics.
