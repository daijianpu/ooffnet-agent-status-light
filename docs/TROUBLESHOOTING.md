# Troubleshooting

Use this checklist before opening an issue.

## The light does not change

1. Confirm that the OOFF NET application is running.
2. Confirm that the physical device is connected and online.
3. Confirm that the adapter ID is registered and bound to the intended light.
4. Send a minimal test state with a valid public state value.
5. Check the response for `ok: true` or `queued: true`.
6. Confirm that the adapter process is not blocking the agent when the light is
   unavailable.

Do not treat a successful device signal test as proof that a real agent hook is
installed. The strongest evidence is a real lifecycle event producing a state
message from the adapter.

## The adapter is discovered but not active

Discovery only means that the manifest is visible. It does not mean that the
agent hook, plugin, or adapter process has been installed, trusted, enabled,
or invoked. Complete the agent's own installation or trust step, then start a
new real turn.

## The state is wrong

Record the smallest event sequence that demonstrates the problem, for example:

```text
turn started → WORKING
permission requested → WAITING
turn completed → COMPLETED
```

Check that the adapter is not inferring state from a process name, emitting a
state from a stale session, or allowing an old turn to overwrite a newer turn.

## Safe issue report

Include operating system, agent version, adapter version, and sanitized state
sequence. Remove prompts, responses, source code, credentials, full paths, and
raw private logs. Use the repository's bug-report form for the remaining
details.
