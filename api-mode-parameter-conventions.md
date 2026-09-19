# Wireshark API Mode-Parameter Conventions

This file records durable conventions for APIs whose callers select among semantic operation modes. Current upstream source remains authoritative.

## Prefer named semantic modes over combinations of booleans

When two or more booleans jointly encode which operation an API should perform, the call site becomes difficult to read and permits combinations that may be meaningless. Prefer a named mode or bit-flag domain whose values state the operation directly and can be extended without adding another positional boolean.

Merged master MR !16531, authored and merged by Guy Harris, replaces PIDL policy-handle `is_open` and `is_close` boolean parameters with semantic flags such as `PIDL_POLHND_OPEN` and `PIDL_POLHND_CLOSE`, with ordinary use represented explicitly by the zero/default mode. The accepted interface makes the generated-code operation visible at the call site rather than requiring readers to remember the meaning and valid combinations of two booleans.

**Implementation rule:** if positional booleans represent a single conceptual mode, model that domain directly with named enum/flag values. Reserve independent booleans for genuinely orthogonal yes/no properties. Make invalid or nonsensical combinations structurally harder to express when the API can do so cleanly.

**Confidence:** Extremely high. Merged API cleanup authored and merged by Guy Harris.
