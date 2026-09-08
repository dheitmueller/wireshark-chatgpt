# Wireshark Filesystem and Archive Path Conventions

This file records durable conventions for filesystem path handling and archive import security. Current upstream APIs and platform behavior remain authoritative.

## Windows-facing paths may legitimately contain either separator

Do not infer path structure with string suffixes that assume one platform separator when the runtime/toolchain can produce either form. This is especially visible in MSYS2, where Python, GLib, Windows APIs, and environment variables can contribute different separator styles to the same workflow.

Merged MR !26349, authored and merged by John Thacker, normalizes path separators in a test because the MSYS2 CPython fork exposes `os.sep` / `os.altsep` differently from assumptions elsewhere in Wireshark. Merged follow-up !26350 replaces a string suffix check for a trailing `bin` component with Wireshark's basename helper, which accepts either path separator.

**Implementation rule:** when asking a structural path question such as “what is the final component?”, use the filesystem/path abstraction that understands all accepted separators rather than parsing the path as an ordinary string. Normalize only at an intentional boundary where one representation is required.

**Confidence:** Very high. Two merged master portability fixes authored and merged by John Thacker.

## Preserve archive metadata width until after security limits are checked

Security limits can be bypassed if an untrusted archive size is narrowed before comparison. A ZIP64 entry exposes a 64-bit uncompressed size; casting it to a signed 32-bit `int` can turn a very large entry negative or otherwise below an intended maximum-size test.

Merged MR !26362, authored by John Thacker and approved/merged by Anders Broman, keeps the ZIP entry size as `uint64_t` through the profile-import acceptance check rather than narrowing it to `int`. The same hardening was accepted for release branches in !26368 and !26369, both merged by Gerald Combs.

**Implementation rule:** carry untrusted length/size metadata in a type that can represent the source format's full range, apply policy and implementation bounds in that representation, and narrow only after the value is proven representable in the destination type.

**Confidence:** Extremely high. Merged master security fix with stable backports merged by project lead Gerald Combs.

## Normalize an archive entry before checking that it remains inside the extraction root

Containment checks must be applied to the path that will actually be used for extraction. Absolute-path compatibility rewriting, separator cleanup, and `..` removal therefore have to precede the final root-containment test. The path API also matters: canonicalization normally requires the target to exist, while archive extraction checks must reason about paths that do not exist yet.

Merged MR !26362 establishes a canonical existing extraction root, rewrites legacy malformed Windows absolute profile paths into a relative form for compatibility, then uses `QDir::cleanPath()` on the prospective full entry path and verifies that it still begins inside the extraction root. The implementation explicitly notes that the not-yet-extracted file cannot be validated using canonical-file APIs and that absolute-path helpers do not themselves guarantee removal of `..` components. The same design was backported in !26368 and !26369.

**Implementation rule:** establish the trusted root with an API appropriate for an existing directory; normalize/rewrite each untrusted prospective child path using semantics that operate on non-existent paths; then perform the containment check. Any legacy compatibility exception must feed back into the same universal containment check rather than bypassing it.

**Confidence:** Extremely high. Merged master path-traversal hardening approved by Anders Broman and subsequently backported under Gerald Combs.
