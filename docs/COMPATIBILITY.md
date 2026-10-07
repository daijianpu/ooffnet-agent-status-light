# Compatibility

This page records the current public validation status. It is intentionally
conservative: a target remains `Testing` until a real integration has been
validated on the stated platform.

## Platform and agent matrix

| Target | Status | Notes |
| --- | --- | --- |
| Windows 10 | ✅ Verified | Primary supported platform for the local OOFF NET application |
| Windows 11 | ✅ Verified | Primary supported platform for the local OOFF NET application |
| macOS | 🧪 Testing | No public release claim yet |
| Linux | 🧪 Testing | No public release claim yet |
| OpenAI Codex | ✅ Verified | Codex lifecycle integration has been validated on Windows |
| Claude Code | 🧪 Testing | Adapter work exists; broader real-world validation is in progress |
| Cursor | 🧪 Testing | Adapter work exists; broader real-world validation is in progress |
| DeepSeek | 🧪 Testing | Adapter work exists; broader real-world validation is in progress |

## Meaning of the labels

- **Verified** — a real integration path has been tested and the expected
  state transitions were observed.
- **Testing** — an adapter or investigation may exist, but the public support
  claim is not yet complete.
- **Planned** — the target is identified but no supported integration is
  available.
- **Not currently supported** — the target is outside the current contract.

## Version and behavior caveat

Agent providers can change lifecycle events, hook permissions, plugin loading,
or desktop behavior without changing their product name. An adapter should
therefore detect the available integration mechanism and fail safely when the
expected lifecycle event is unavailable.

Do not infer agent activity from a process name alone. A desktop process being
open is not proof that a conversation is working, waiting, or complete.

## Reporting a compatibility result

Include only the minimum information needed to reproduce the result:

- operating system and architecture;
- agent product and version;
- adapter version or commit;
- the state sequence observed;
- whether the physical light matched the sequence;
- sanitized error text, if any.

Do not attach prompts, responses, source code, credentials, or private logs.
