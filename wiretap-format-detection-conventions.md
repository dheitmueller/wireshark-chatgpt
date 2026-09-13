# Wireshark Wiretap Format-Detection Conventions

This file records durable conventions for capture-file format probing and open-path diagnostics. Current upstream Wiretap behavior remains authoritative.

## Treat normal format rejection as probe behavior, not a warning

Wiretap attempts multiple format openers while identifying a file. An opener discovering that the input is not its format is therefore an expected branch of format detection, not necessarily an operational failure worthy of user-visible warning-level logging.

Merged master MR !21215, authored, approved, and merged by Michael Mann, lowers several K12 open-path messages from `ws_warning()` to `ws_debug()` because short headers, bad magic, and related checks are routinely encountered while K12 probes files of other formats. The opener still returns the appropriate `WTAP_OPEN_NOT_MINE` or error result; only the diagnostic severity changes.

**Implementation rule:** distinguish ordinary probe rejection from a failure after a format has been recognized. Conditions that merely establish "not my format" should normally be silent or debug-level; reserve warnings/errors for unexpected failures or malformed data after ownership of the input format is established. This avoids log spam during normal multi-format detection while preserving useful diagnostics for real failures.

**Confidence:** High. Merged master change authored and merged by Michael Mann with the normal-probe rationale stated explicitly.