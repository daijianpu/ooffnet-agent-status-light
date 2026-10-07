# Adapter Examples

This directory contains contract-level examples only. It does not contain an
SDK, installer, executable, hook implementation, or production source.

## Minimal message

An adapter reports a state with one UTF-8 JSON line:

```json
{"type":"agent_state","adapter_id":"example.agent","state":"WORKING"}
```

## Conceptual event mapping

```text
agent turn accepted   → send WORKING
agent requests input  → send WAITING
agent turn completed  → send COMPLETED
agent fails           → send ERROR
agent recovers        → send RECOVERING
agent becomes inactive→ send IDLE
```

Use the actual lifecycle or hook API of the agent you integrate. Do not copy
private OOFF NET implementation files into an adapter, and do not send
conversation content through the state message.

## Before publishing an adapter

- Use a stable, unique `adapter_id`.
- Use only the six public Agent states.
- Make delivery fail-open so the agent is never blocked by the light.
- Avoid logging sensitive data.
- Test a real lifecycle event, not only a manually generated signal.
- Document the agent and platform versions that were actually tested.
