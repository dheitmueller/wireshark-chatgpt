# Wireshark Single-Pass Analysis Conventions

This file records durable conventions for dissectors and taps that must work both when packets can be revisited and when a frontend consumes the capture in one pass. Current upstream source remains authoritative.

## Do not make one-pass output depend on information that can only be known later

A two-pass or interactive workflow can revisit earlier packets after later packets have established completeness or other derived state. A one-pass streaming frontend such as TShark cannot. Code shared by those execution models must not require future packets to have already established a fact before it preserves the data needed to produce the final result.

Merged master MR !14901, authored by John Thacker and merged by Gerald Combs, fixes TFTP Export Objects in single-pass TShark. The old path copied transfer blocks only after `last_package_available` was known, but during the first sequential pass that fact cannot be known until the final block is encountered; earlier blocks had therefore already been discarded. The accepted implementation preserves blocks during the first pass while no missing blocks are known, grows the object in file-scope memory, and only relies on the known final size on a visited/later pass. Release MRs !14908, !14909, and !14910 carry the same behavior to supported branches.

**Architecture rule:** when a tap/export/statistics result may be produced in a one-pass frontend, identify which predicates require future packets. Preserve the minimum state needed to complete the result before those predicates become knowable rather than discarding early input and assuming redissection will recover it.

**Lifetime rule:** speculative or partially accumulated data that might never be emitted should use a Wireshark-managed scope matching the analysis lifetime when that avoids manual cleanup on abandoned/incomplete paths. Do not trade one-pass correctness for leaks or fragile conditional frees.

**Review rule:** test stateful tap/export behavior in both one-pass and revisit/two-pass execution models when the code branches on `visited` state or on facts learned only at end-of-transfer/end-of-capture.

**Confidence:** Very high. Merged John Thacker master correctness fix with three accepted stable backports and an execution-model explanation in the change itself.