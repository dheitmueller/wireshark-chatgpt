# Wireshark Wiretap File-Probing Conventions

This file records durable conventions for capture-file format probing and low-level input APIs in Wiretap. Current upstream Wiretap behavior remains authoritative.

## Preserve stdio-style sentinel values until they have been classified

Character-oriented APIs such as `fgetc()` return an `int` specifically so every byte value can coexist with an out-of-band sentinel such as `EOF`. Casting that result to `char` before checking the sentinel collapses those domains: behavior then depends on whether plain `char` is signed, and a legitimate byte can be confused with end-of-file or vice versa.

Merged master MR !13461, authored and merged by John Thacker, fixes the EMS capture-file reader by keeping the results of `file_getc()`/`file_peekc()` as `int` until EOF has been checked. The old code could produce format false positives on signed-char platforms and false negatives on unsigned-char platforms.

**Implementation rule:** preserve the full return type of an API that combines data with a sentinel until the sentinel/error state has been classified. Only narrow to the protocol's byte type after proving that the returned value represents data.

**Portability rule:** do not rely on the implementation-defined signedness of plain `char` when classifying byte values or EOF-like sentinels. A parser that works only under one `char` model is not portable enough for Wiretap.

## During format sniffing, distinguish ordinary EOF from an I/O failure

A file-format opener is often probing whether an input belongs to its format. Reaching EOF while trying to read a required magic/header byte can therefore mean "not my file format" rather than "the file could not be read." Conversely, a genuine underlying read error must not be silently reclassified as a format mismatch.

The same merged !13461 change checks `file_eof()` after a negative peek/read result. Ordinary EOF returns `WTAP_OPEN_NOT_MINE`, while a real read failure reports `WTAP_OPEN_ERROR`. This also prevents misleading diagnostics such as "file could not be opened: Success" when the actual condition was merely an incomplete/nonmatching file.

**Format-probing rule:** classify short input according to the opener contract. If the probe simply lacks enough bytes to establish the format, return the format-mismatch result; if the underlying I/O operation itself failed, preserve that as an I/O error.

**Review rule:** for every file-type sniffer, inspect the first reads separately from normal in-format parsing. Verify the code distinguishes: complete matching header, complete nonmatching header, EOF before enough bytes are available, and a genuine read error.

**Testing rule:** include zero-length and truncated inputs, high-bit byte values on paths that use character-oriented I/O, and injected/read-error cases where practical. Run on or otherwise exercise both signed- and unsigned-`char` assumptions when a parser has historically stored stdio results in `char`.

**Confidence:** Very high. Merged master Wiretap correctness/portability fix authored and merged by John Thacker, with the signedness failure modes and EOF-vs-I/O distinction described directly in the accepted change.

## Put a conservative work budget on speculative format detection

A Wiretap opener can run merely because a file is being considered, before Wireshark knows that the format belongs to that opener. A weak heuristic therefore must not use the format's maximum legal file size as permission to scan arbitrarily far into an unrecognized file.

Merged master MR 4344, authored by John Thacker, fixes CAM Inspector and Ixia VeriWave probes that could read an enormous candidate file while `wtap_open_offline()` was still determining its type. The same change also moves the CAM Inspector accounting to a width that cannot overflow when evaluating a large candidate. The accepted implementation introduces a finite probe bound. Direct review from Guy Harris says even the historical 1 GiB bound could probably be cut back much further, suggesting roughly 1 MiB or 16 MiB.

**Format-probing rule:** impose an explicit work/byte budget on speculative recognition, independent of the largest file the committed reader can handle. The budget should be only as large as needed to distinguish the format with acceptable confidence.

**Arithmetic rule:** counters used to score a bounded or unbounded probe must still be wide enough for the maximum values and intermediate expressions they can reach.

**Review rule:** treat the numeric threshold in MR 4344 as historical, not normative. Guy's review is strong evidence that reviewers should challenge an apparently bounded probe that can still stall interactive callers for an unreasonable amount of time.

**Confidence:** Extremely high for the bounded-probe principle: merged master correctness/performance fix by John Thacker plus direct Guy Harris review. The exact byte budget remains format-specific.
