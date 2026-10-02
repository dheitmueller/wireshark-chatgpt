# Heuristic and source-range conventions from MRs 1360-1409

Merged master !1364, with stable backport !1386, validates several independent QUIC structural invariants before creating or binding persistent conversation state. Heuristic probing should remain free of long-lived state changes until recognition succeeds.

Merged !1360 fixes SOME/IP-SD generated fields that used an offset after the parser had advanced through an entire entry. Preserve an immutable structure-start offset separately from the mutable parse cursor whenever later items must refer to the enclosing wire range.

Merged !1393 reinforces that alignment and padding loops are ordinary bounds-sensitive parsing: verify an index or remaining range before decrementing or consuming it.
