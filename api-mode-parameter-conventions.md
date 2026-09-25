# Wireshark API Mode-Parameter Conventions

This file records durable conventions for APIs whose callers select among semantic operation modes. Current upstream source remains authoritative.

## Prefer named semantic modes over combinations of booleans

When two or more booleans jointly encode which operation an API should perform, the call site becomes difficult to read and permits combinations that may be meaningless. Prefer a named mode or bit-flag domain whose values state the operation directly and can be extended without adding another positional boolean.

Merged master MR !16531, authored and merged by Guy Harris, replaces PIDL policy-handle `is_open` and `is_close` boolean parameters with semantic flags such as `PIDL_POLHND_OPEN` and `PIDL_POLHND_CLOSE`, with ordinary use represented explicitly by the zero/default mode. The accepted interface makes the generated-code operation visible at the call site rather than requiring readers to remember the meaning and valid combinations of two booleans.

**Implementation rule:** if positional booleans represent a single conceptual mode, model that domain directly with named enum/flag values. Reserve independent booleans for genuinely orthogonal yes/no properties. Make invalid or nonsensical combinations structurally harder to express when the API can do so cleanly.

**Confidence:** Extremely high. Merged API cleanup authored and merged by Guy Harris.

## Split boolean-selected questions into named predicates when callers are really asking different questions

A boolean parameter is a poor fit when it does not modify one operation but instead chooses between two semantically distinct queries. Give each query a name so call sites state the question directly and cannot accidentally invert the boolean.

Merged master MR !9625 replaces `extcap_has_configuration(ifname, is_required)` with separate `extcap_has_configuration(ifname)` and `extcap_requires_configuration(ifname)` predicates. The former answers whether an extcap exposes any configurable options; the latter answers whether capture must stop for missing required configuration. This change also fixes the important case where a mandatory parameter already has a usable default value and therefore does not require user intervention.

**API rule:** if a positional boolean selects which semantic predicate an API evaluates, prefer separate named predicates (or a named mode when there are several extensible cases) over requiring every caller to remember the meaning of `true` and `false`. Reserve boolean arguments for genuinely orthogonal yes/no properties of one operation.

**Confidence:** Very high. Merged master API cleanup; the new names are used directly by the Qt capture-option call sites and make the two policies explicit.
