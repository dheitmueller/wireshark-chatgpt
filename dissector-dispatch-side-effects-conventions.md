# Wireshark Dissector Dispatch Side-Effect Conventions

Current upstream source remains authoritative.

## Match table-dispatch behavior to the logical protocol layering

Dissector-table lookup helpers can differ in more than lookup mechanics; they can also affect frame protocol-list or presentation state. Select the dispatch API whose side effects match the logical layering rather than treating all lookup helpers as interchangeable.

Merged master MR !10305, authored by John Thacker, changes nested DRDA command and parameter dispatch from `dissector_try_uint()` to `dissector_try_uint_new(..., FALSE, NULL)`. The stated reason is that nested DRDA dispatch should not add the DRDA protocol name to the frame's protocol list repeatedly.

**Implementation rule:** for nested dispatch that remains inside one logical protocol layer, verify whether the chosen table-dispatch helper records another protocol-layer occurrence. Use the API form that suppresses such bookkeeping when the nested parser is only a code-point-specific implementation detail.

**Review rule:** when changing between `dissector_try_*` variants, inspect not just the return value and table key but also the API's protocol-list/tree side effects. A functionally successful subdissector call can still produce misleading protocol-stack presentation if the wrong dispatch form is used.

**Confidence:** Very high. Merged master change by John Thacker with the side-effect rationale stated explicitly.

## Reuse one handle when multiple keys have one decode contract

Merged master MR !10312, also authored by John Thacker, creates one `sqlstt_handle` and registers it for both DRDA SQLSTT and SQLATTR because both code points carry the same FD:OCA SQLSTTGRP data object.

**Implementation rule:** when multiple table keys intentionally share an identical payload contract, register the same dissector handle for those keys. This makes the shared contract visible at registration time and avoids redundant handles around the same parser.

**Confidence:** High. Merged master implementation by John Thacker.
