# Wireshark Packet-Column Conventions

This file records durable conventions for composing packet-list column text across nested dissectors. Current upstream column APIs and dissector behavior remain authoritative.

## Nested dissectors should preserve meaningful Info text from their caller

When a subdissector contributes additional semantic detail to the Info column, overwriting the caller's already-useful text can erase the encapsulation or command context that explains the nested payload. Prefer an append operation with an explicit separator when both layers are useful to the user.

Merged master MR !23509 changes the GSM SIM APDU dissector to use `col_append_sep_fstr()` for command and response descriptions rather than replacing existing `COL_INFO` text. The caller-specific separator previously inserted by the AT dissector is removed, making the nested dissector responsible for composing its own contribution consistently. The accepted release-4.6 backport is !23525.

**Implementation rule:** if a dissector can be invoked as a nested decoder and its Info text supplements rather than supersedes the caller's description, append with a standard separator instead of using a replace-style column API. Keep composition responsibility with the dissector adding the text rather than requiring every caller to pre-seed separators.

**Confidence:** High. Merged master behavior with an accepted supported-branch backport; no contrary review discussion.