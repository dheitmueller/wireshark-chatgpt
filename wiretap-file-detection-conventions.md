# Wireshark Wiretap File-Detection Conventions

This file records durable conventions for capture/file-format recognition extracted from accepted Wireshark review. Current upstream source and documentation remain authoritative.

## Order file recognizers by confidence so weak heuristics cannot preempt stronger formats

Wiretap file-opening probes are not independent when they are tried sequentially: an early false positive can prevent the correct later recognizer from seeing the file. Detection order is therefore part of the correctness contract, especially for formats whose opener uses only weak structural heuristics rather than a strong magic value or highly specific syntax.

Merged master MR !22467, authored and merged by John Thacker, moves the Ixia IxVeriWave and CAM Inspector openers to the end of `open_info_base[]`. The source already described them as "extremely weak heuristics"; John reported real false positives on otherwise valid JSON. More specific JSON-derived formats remain before the generic JSON reader, while these especially weak format probes are placed after stronger/magic-based recognizers.

**Implementation rule:** when registering or reordering wiretap file recognizers, consider both each recognizer's confidence and interactions with formats that can satisfy the same superficial checks. Put strong magic/signature and narrowly constrained recognizers before broad or weak heuristics; put recognizers known to produce plausible false positives late enough that they cannot steal unrelated files. Treat recognizer order as behavioral logic, not cosmetic table organization.

**Review implication:** a new or modified heuristic opener should be evaluated not only with positive samples of its own format but also against plausible competing/common formats whose contents can accidentally satisfy the heuristic.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, motivated by observed false identification of valid JSON files.

## Keep speculative file probes quiet until the opener establishes ownership

A wiretap opener can invoke a general-purpose parser while it is still only asking whether the file belongs to that format. Parse failures on unrelated files are therefore normal probe outcomes, not user-facing errors. A recognizer that ultimately returns `WTAP_OPEN_NOT_MINE` should not leak dependency diagnostics to stderr merely because a library tried and rejected the input.

Merged master MR !22240, authored and merged by John Thacker, adds `XML_PARSE_NOERROR` while the 3GPP TS 32.423 opener asks libxml2 to parse a candidate file. Before that change, opening an unrelated capture could print libxml2 syntax errors even though another wiretap reader would subsequently accept the file. The opener still returns `WTAP_OPEN_NOT_MINE` when parsing fails; only the premature diagnostic side effect is suppressed.

**Implementation rule:** while a file opener is in recognition/probing mode, suppress or capture diagnostics from subordinate parsers unless and until the opener has established that the input is its format. Report meaningful parser errors after ownership is known, but keep ordinary `OPEN_NOT_MINE` paths observationally quiet.

**Confidence:** Very high. Merged master correctness/UX fix authored and merged by John Thacker with a minimal direct change to the recognition path.

## Do not create persistent reader state while merely probing a candidate file

Format recognition and committed reading can legitimately reuse the same parser, but the probe path must not perform mutations that assume the file has already been accepted. Per-file interface tables, IDBs, conversation-like mappings, and similar state belong after recognition establishes ownership.

Merged master MR !20434, authored and merged by Guy Harris, deliberately passes a NULL `wtap *` into `candump_parse()` during `candump_open()`. The accompanying comment explains that the reader has not yet decided the file is candump and has not initialized its interface-name table, so the probe must not try to look up interface names or create interfaces. The explicit comment was added to prevent a future cleanup from treating the NULL as an accidental omission.

**Implementation rule:** separate speculative parsing from committed side effects. If one parsing helper serves both phases, make the ownership/context argument explicitly optional and gate persistent mutations on it, or split probing from committed parsing when that is clearer. Document intentional sentinel/NULL context where it protects a subtle lifecycle boundary.

**Confidence:** Extremely high. The convention comes directly from a merged master change authored and merged by Guy Harris whose sole purpose was documenting this intentional probe/commit distinction.

## Carry detected file-family identity into the record framer

When a wiretap opener accepts more than one related byte-stream family, a successful recognition should establish which **record grammar** the reader will use. Repeatedly trying alternate unrelated packet framing rules at every record can misclassify valid padding or ordinary payload bytes as another container type.

Merged John Thacker MR !10342 separates MPEG audio from MPEG PES reading and stores the detected `is_audio` mode in the wiretap private context. The PES path then allows zero-byte stuffing before the next start code without treating it as an audio packet. Earlier merged !10322 makes the MPEG `program_end_code` a four-byte terminator without a payload-length field, illustrating that terminal markers need their own framing case.

Companion merged !10331 documents that MPEG's short magic numbers are weak enough to keep the opener in `OPEN_INFO_HEURISTIC` despite recognizable bytes, and !10327 synchronizes extensions across both open-dialog filtering and heuristic registration.

**Implementation rule:** make the opener's format decision explicit in per-file state and use it consistently for subsequent record framing; handle format-defined stuffing and terminators separately from record payloads. Treat recognizer confidence and filename extensions as supporting discovery signals, not as interchangeable parsers.

**Confidence:** High — merged John Thacker wiretap changes with explicit failure modes, plus a compatible registration correction.

## Register both file extension metadata and the actual opener

Merged master !9725 updates Wiretap `file_access.c` documentation: every added file type, even extensionless formats, must appear in `open_info_base[]`, not only the extensions list. Reader probes return `WTAP_OPEN_ERROR`, `WTAP_OPEN_MINE` or `WTAP_OPEN_NOT_MINE`; the cleanup contract for reader-private state differs between errors managed by `wtap_close` and NOT_MINE probes preceding another opener.

**Rule:** Review both format-discovery registrations, named open outcomes and cleanup paths together. **Confidence:** High; merged documentation of concrete Wiretap contracts.

## Normalize ASCII file extensions once before case-insensitive heuristic lookup

Merged master !9594 and Guy Harris's companion backports !9598/!9599 make Wiretap extension-assisted recognition case-insensitive. Pascal Quantin asked whether `g_ascii_strcasecmp()` could avoid an extra allocation; the accepted implementation used the project's `ascii_strdown_inplace()` and compared against lowercase registered extensions, so normalization happens once rather than in every iteration.

**Rule:** for file-format extension matching, use locale-independent ASCII comparison or normalized ASCII lookup keys consistently across the extension table and query; avoid unnecessary temporary allocations in repeated probes. Case-insensitive extension matching does not turn a weak file-format heuristic into a strong magic-based identification. **Confidence:** very high; accepted master and Guy Harris backports, explicit Pascal Quantin review.
