# Wireshark Configuration File I/O Conventions

This file records durable conventions for reading, copying, and writing Wireshark configuration files. Current upstream source remains authoritative.

## Validate the expected filesystem object type, not just pathname existence

A path existing is weaker than the contract required by code that intends to read or copy a normal configuration file. Directories, FIFOs, device nodes, and other special objects can occupy a configuration pathname and should not automatically be treated as files merely because `stat()` or an existence test succeeds.

Merged master MR !14471, authored and merged by John Thacker, adds `test_for_regular_file()` and changes profile configuration copying to use it rather than `file_exists()`. The helper deliberately uses `stat()` semantics, so a symlink resolving to a regular file satisfies this particular contract; the important distinction is regular-file content versus a directory or special object. The motivating cases include a directory, FIFO, or block device manually placed under a preference filename.

**Implementation rule:** choose a filesystem predicate that matches the operation's semantic precondition. Code that copies or parses a normal configuration file should verify that the resolved object is a regular file rather than treating generic existence as sufficient. Do not rely on a later open/copy failure to define ordinary control flow for known wrong object types.

**Review rule:** for configuration/profile migration code, test at least a directory and one special-file case in addition to missing and ordinary files when platform facilities make that practical. Be explicit if symlinks are followed or rejected; that is a separate policy decision from the regular-file check.

**Confidence:** Very high. Merged master filesystem/configuration robustness change authored and merged by John Thacker.

## Do not let failure of an auxiliary preferences file suppress the primary preferences write

When preferences are intentionally split across more than one persistence file, each file has its own failure boundary. Failure to create an optional or auxiliary file should be reported appropriately, but it should not prevent unrelated primary preferences from being saved unless the operation is explicitly transactional across all files.

Merged master MR !14470, authored and merged by John Thacker as a follow-up to the extcap preference split, changes `write_prefs()` so inability to open the separate `extcap` preferences file warns and skips that auxiliary write but still proceeds to write the main preferences file. The same change treats a directory occupying a module-preference pathname as unusable for that module and falls back to the generic preferences file rather than aborting the preference read path.

**Implementation rule:** define which configuration artifacts are primary, auxiliary, and transactionally coupled. For independent files, isolate errors so one failed auxiliary read/write does not discard or suppress valid work on the primary file. Preserve useful diagnostics for unexpected failures while allowing deliberately ignorable wrong-object cases to fall back cleanly.

**Review rule:** when configuration is split into new files, audit both read fallback and write error propagation. A successful refactor must preserve the durability of existing primary settings even when the new file cannot be created, is absent, or is shadowed by an unusable filesystem object.

**Confidence:** Very high. Merged master follow-up authored and merged by John Thacker, directly correcting failure propagation introduced by preference-file separation.