# Wireshark UI Option Value Conventions

This file records durable conventions for mapping persistent semantic settings to GUI widgets. Current upstream UI code remains authoritative.

## Do not use presentation indexes as persistent semantic values

A combo-box row index is a property of the current presentation order, not a stable identity for an option. Different widgets can expose different subsets or supersets of the same semantic enum, and entries can be reordered for usability without changing the underlying setting.

Merged master MR !12248, authored and merged by John Thacker, unifies the conceptual `Show as` / `Show data as` setting used by Show Packet Bytes and Follow Stream. Show Packet Bytes exposes a superset of the values supported by Follow Stream, so the enum value can no longer be assumed to equal the combo-box index. The accepted implementation stores the enum in item data, uses `findData()` when mapping a persisted value back to a row, and reads the semantic value through `currentData()`.

**Implementation rule:** keep the persistent/configuration value in a stable enum or key and attach that value to the UI item as data. Convert semantic value to presentation index with lookup (`findData()` or an equivalent keyed mapping), and convert the selected item back through its stored data. Do not serialize or compare row indexes unless index ordering is itself the defined external contract.

**Review rule:** whenever two widgets share an enum but present different subsets, or when a list is alphabetized/reordered, explicitly verify that no code still assumes `index == enum`. Handle unsupported semantic values deliberately rather than allowing an accidental row to become selected.

**Confidence:** Very high. The rule comes directly from a merged master change authored and merged by John Thacker, whose MR description explicitly identifies breaking index/enum correspondence as an intentional design improvement.