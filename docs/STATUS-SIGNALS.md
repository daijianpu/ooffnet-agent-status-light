# Status Signals

The public Agent contract has six states. Adapters must use these exact
uppercase values and must not invent additional Agent states.

## Agent states

| Value | Physical signal | Meaning |
| --- | --- | --- |
| `IDLE` | Green steady | The adapter has no active work to report |
| `COMPLETED` | Green blinking | The current turn completed successfully |
| `WORKING` | Yellow steady | The agent is actively processing work |
| `WAITING` | Yellow blinking | The agent is waiting for user input, permission, or another action |
| `RECOVERING` | Red steady | The agent is attempting to recover from a transient problem |
| `ERROR` | Red blinking | The agent encountered an error or needs attention |

The exact completion-hold duration and device rendering are implementation
details of the OOFF NET application. An adapter should report the lifecycle
event and should not attempt to reproduce the light animation itself.

## Device and connection signals

These are device-level signals, not Agent states:

| Condition | Physical signal |
| --- | --- |
| `CONNECTING` | Red → yellow → green sequence |
| `COMMUNICATION_ERROR` | All three lights blinking |
| `FATAL_ERROR` | All three lights on |
| `IDENTIFY` | Fast three-light blink |

Device conditions are owned by the OOFF NET application and hardware. Adapter
authors should not send them through the Agent-state contract.

## State selection guidance

- Report `WORKING` when the agent has accepted work and is actively processing
  it.
- Report `WAITING` when the user must provide input, permission, or an
  explicit decision before progress can continue.
- Report `COMPLETED` when the current turn has completed.
- Report `ERROR` when the agent has failed or requires intervention.
- Report `RECOVERING` only when the agent is actively attempting a recovery
  path, not merely because a transient event was observed.
- Report `IDLE` when the session is not performing work and no attention is
  required.

The adapter remains responsible for mapping its own events to these meanings.
The OOFF NET application does not inspect prompts, responses, or tool payloads
to guess a state.
