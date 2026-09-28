# Review findings: Wireshark MRs !6661-!6710

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are treated as stronger evidence than closed submissions. Maintainer-authored changes and direct maintainer review, especially Guy Harris and John Thacker, receive correspondingly higher weight.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !6710 | merged | Scanned | release-3.4 CQL backport uses `ENC_TIME_USECS` so the protocol timestamp is formatted in its actual microsecond unit; authored by Guy Harris as a cherry-pick. |
| !6709 | merged | Scanned | release-3.6 counterpart of the CQL microsecond timestamp fix. |
| !6708 | merged | Deep | Guy Harris stable-branch backport adds the `ENC_TIME_USECS` core timestamp encoding and documentation, including microseconds-to-`nstime` conversion and related time-encoding doc corrections. |
| !6707 | merged | Deep | release-3.6 counterpart of the core `ENC_TIME_USECS` implementation; same accepted API contract. |
| !6706 | merged | Scanned | Guy Harris corrects time-encoding documentation, including the NTP epoch year and redundant wording. |
| !6705 | merged | Scanned | Reverts a stable-branch documentation change that was already present; useful release-branch hygiene but no new convention. |
| !6704 | merged | Scanned | release-3.4 documentation for factored-out time encodings. |
| !6703 | merged | Scanned | release-3.6 documentation for factored-out time encodings. |
| !6702 | merged | Scanned | Master CQL change consumes the new `ENC_TIME_USECS` API for the default timestamp. |
| !6701 | merged | Scanned | CIP Safety refactor extracts repeated format decoders ahead of a later correctness fix; review caught indentation. |
| !6700 | merged | Deep | Gerald Combs fixes BACapp recursion accounting by routing an early exit through common cleanup so the protocol-depth decrement always matches the increment. |
| !6699 | merged | Deep / high-authority | Gerald Combs moves systemd journal recognition ahead of IxVeriWave after a false positive; Guy Harris explicitly characterizes the IxVeriWave heuristic as extremely weak, strongly confirming confidence-ordered wiretap probing. |
| !6698 | merged | Scanned | WSLua menu-group documentation is updated after the dynamic statistics-group refactor in !6680. |
| !6697 | merged | Scanned | Falco Bridge cleanup makes plugin headers explicit in CMake and consolidates internal definitions. |
| !6696 | merged | Scanned | Pascal Quantin works around GCC 10.2.1 compilation behavior in generated NGAP code. |
| !6695 | merged | Deep | João Valverde adds variadic display-filter min/max functions, extending AST/compiler/VM calling convention, docs and tests; John Thacker tests repeated-field and byte-ordering behavior, and the implementation reuses ordinary comparison semantics. |
| !6694 | merged | Discussion-focused | Adds interface-type filtering for Logwolf; Gerald Combs and Roland Knall discuss capability-based extcap reuse versus product-specific directories, while John Thacker catches source hygiene issues. |
