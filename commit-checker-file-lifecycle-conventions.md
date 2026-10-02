# Wireshark Commit-Checker File Lifecycle Conventions

## Treat deletion and rename as normal inputs to history-scoped checkers

A checker that selects files from a commit range can encounter paths that no longer exist at the range's final tree. Deletion is part of the change being reviewed, not an exceptional filesystem failure.

Merged master MR !1533, authored by Martin Mathieson, adds explicit existence checks to `tools/check_spelling.py`, `tools/check_tfs.py`, and `tools/check_typed_item_calls.py`. The change was prompted by checker exceptions when files named by recent commits had been deleted.

**Tooling rule:** history or commit-scoped validators must tolerate paths deleted or renamed by the commits they inspect. Skip or report an absent final-tree path cleanly rather than throwing before the remaining changed files can be checked.

**Review rule:** when adding a checker mode based on `--commits` or another Git-derived file set, test a range that adds, modifies, renames, and deletes files.

**Confidence:** Very high. Merged master tooling hardening authored by Martin Mathieson across three project checkers.
