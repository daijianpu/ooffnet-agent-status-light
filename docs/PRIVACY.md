# Privacy Boundary

OOFF NET's public adapter contract is designed for local lifecycle signalling,
not for collecting agent conversations.

## Data an adapter may report

- a registered adapter ID;
- one of the six public Agent states;
- optional short identifiers such as a session ID or turn ID, when the agent
  provides them and they are safe to use locally;
- a short hook or source name for diagnostics.

## Data an adapter must not report

Do not send or store through this contract:

- prompts or model responses;
- source code or file contents;
- tool arguments or tool outputs;
- API keys, access tokens, cookies, or passwords;
- personal data that is not necessary for local diagnostics;
- full local paths or customer logs;
- hidden chain-of-thought or internal reasoning.

The OOFF NET application does not need those values to show a physical status
signal.

## Local-first behavior

The adapter endpoint is local to the user's computer. This public repository
does not define a cloud telemetry service, account requirement, or remote data
collection path. A separate product or integration must document and obtain
consent for any additional data flow it introduces.

When requesting support, sanitize diagnostic output before sharing it publicly.
Use the smallest reproducible state sequence instead of uploading raw logs.
