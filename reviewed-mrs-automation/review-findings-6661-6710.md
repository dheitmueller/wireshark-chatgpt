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
| !6693 | merged | Scanned | John Thacker makes Protocol Hierarchy byte counts include field appendix length. |
| !6692 | closed | Discussion-focused (closed) | Review asks the contributor to split unrelated MIKEY/XML changes into separate MRs and correct submission metadata; the contributor closes and resubmits separately. Workflow evidence only. |
| !6691 | merged | Deep | Master implementation of the microsecond timestamp encoding in the core protocol-field decoder and documentation. |
| !6690 | merged | Scanned | Removes a leftover display-filter test debug print. |
| !6689 | merged | Scanned | CIP Safety presents protocol timing quantities in human-readable milliseconds while retaining underlying field values. |
| !6688 | merged | Deep | CIP date/time arithmetic is widened to 64 bits before day-to-second multiplication, preventing future-date overflow from narrow intermediate arithmetic. |
| !6687 | merged | Scanned | John Thacker changes WHOIS text handling to an explicit UTF-8 assumption, documents protocol charset ambiguity, and reports that assumption with expert info. |
| !6686 | merged | Deep | João Valverde replaces a punctuation-based macro/field heuristic with actual registered-field resolution; semantic identity comes from the registry, not token punctuation. |
| !6685 | merged | Deep | John Thacker makes WHOIS/Finger dissect at FIN or after out-of-order completion on the first pass and gates reassembled TCP data on exact frame and protocol-layer identity. |
| !6684 | merged | Scanned | Fixes a debug-representation macro to honor its allocator/scope argument rather than silently passing NULL. |
| !6683 | merged | Scanned | Adds a generated PER label that explains how to expose PER internal fields. |
| !6682 | closed | Discussion-focused (closed) | Review identifies submission from fork master; protected-branch automation cannot modify it, and the work is later consolidated elsewhere. Branch-hygiene evidence only. |
| !6681 | merged | Scanned | Renames the Logwolf Qt source directory and associated build target from transitional naming. |
| !6680 | merged | Deep | Gerald Combs splits dynamic menu/stat groups by packet versus log product while the WSLua generator preserves deprecated script-visible aliases; !6698 follows with binding documentation. |
| !6679 | merged | Discussion-focused | A narrowing-warning fix exposes the signed-position versus unsigned-size domain problem; Guy Harris notes that size_t is wider than long on Win64, useful portability context but not a universal type prescription. |
| !6678 | merged | Deep | Display-filter compilation represents unavailable error location explicitly with a negative sentinel and suppresses underlining when no trustworthy span exists; macro-expanded locations improve as well. |
| !6677 | merged | Scanned | Synchronizes Debian exported-symbol lists with current library ABI. |
| !6676 | merged | Scanned | Removes packet-specific statistics and utility menu entries from Logwolf. |
| !6675 | merged | Scanned | release-3.4 backport of strict setup-script failure behavior. |
| !6674 | merged | Scanned | release-3.6 backport of strict setup-script failure behavior. |
| !6673 | merged | Deep | Gerald Combs makes Debian/RPM provisioning scripts fail nonzero on errors because they build container images; option variables and optional arguments are initialized safely. |
| !6672 | merged | Scanned | Repairs Qt translations after splitting MainWindow into WiresharkMainWindow/LogwolfMainWindow. |
| !6671 | merged | Scanned | Automatic registry/translation/data refresh. |
| !6670 | merged | Deep | tshark consumes compiler-provided display-filter source spans to underline the exact erroneous expression; later MRs in this batch harden missing and macro-expanded locations. |
| !6669 | merged | Deep | ACN/RDMnet TCP heuristic now runs its protocol predicate before claiming traffic, returning FALSE on mismatch. |
| !6668 | merged | Scanned | Automatic release-3.6 data/translation refresh. |
| !6667 | merged | Scanned | Automatic release-3.4 data/translation refresh. |
| !6666 | merged | Deep / high-authority | John Thacker splits an out-of-order TCP multi-segment PDU after a completed prefix is dissected but an incomplete suffix still needs bytes, preserving frame/layer/proto-data identity across first and subsequent passes and enabling previously skipped regression tests. |
| !6665 | merged | Deep | João Valverde carries display-filter column offset/length from scanner tokens into syntax-tree nodes and compiler errors instead of reconstructing diagnostic positions afterward. |
| !6664 | merged | Scanned | macOS Homebrew setup installs PCRE2 after it becomes a required build dependency. |
| !6663 | merged | Deep / high-authority | Guy Harris directs Bluetooth SCO column handling to keep stable base Info text before packet reads that can throw, then append optional decoded status with the append API; the contributor applies both suggestions before merge. |
| !6662 | merged | Scanned | release-3.4 build logic detects Sparkle's version and requires Sparkle 1 exactly until Sparkle 2 API support exists. |
| !6661 | merged | Scanned | release-3.6 counterpart of the exact Sparkle 1 compatibility gate. |
