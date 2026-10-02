# Wireshark MR review findings: !1510-!1559

Corpus commit used: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 MRs, newest to oldest. Merged work is treated as the strongest implementation evidence; closed !1539 is recorded for accounting but is not used as accepted precedent.

## Durable synthesis

### Keep protocol state split by the layer that owns and resets it — !1556

Merged master MR !1556 originally extended one NR DRB mapping structure across MAC/RLC and RLC/PDCP configuration. Pascal Quantin caught that the structure was cleared when entering `RLC-BearerConfig`, which could destroy state populated under the independent PDCP configuration path. Martin Mathieson agreed that one structure was spanning two different protocol-layer ownership domains and split it into separate MAC/RLC and RLC/PDCP mappings.

**Rule:** group mutable configuration/state by semantic owner and reset boundary. If two protocol subtrees populate state independently, do not put them in one scratch object merely because their identifiers overlap; one subtree's initialization must not erase another layer's state.

### Raise dependency floors only to what every supported platform can supply — !1543

Merged master MR !1543, authored by John Thacker, raises the minimum libgcrypt version from 1.4.2 to 1.5.0 after RHEL/CentOS 6 became unsupported, while explicitly stopping short of 1.6 because RHEL/CentOS 7 supplied 1.5.3. The higher floor then permits deletion of the legacy AES unwrap fallback.

**Rule:** choose a dependency minimum from the oldest dependency version guaranteed by the currently supported platform matrix, not from the newest useful upstream feature. Once the supported floor makes a compatibility path unreachable, remove that path in the same transition when practical.

### Warning cleanup must preserve semantic types rather than merely silence the compiler — !1524, !1546

Merged !1524 received detailed Pascal Quantin review. A warning fix that cast a `double` time result down to `guint32` was redirected so the integer timer was converted to `double`, preserving the higher-precision semantic domain. Pascal also pushed back on unnecessary loss of `const`, suggested widths matching `tvb_get_ntohs()`, and repeatedly favored the least invasive type/cast change consistent with the actual APIs.

Merged !1546 adds the complementary caution. Anders Broman noted that offsets are often signed in Wireshark APIs and some helpers use `-1` as "not found"; mechanically making offsets unsigned because ordinary offsets are non-negative can erase a sentinel contract.

**Rule:** resolve warnings by matching the true value domain and API contract. Preserve constness, precision, signed sentinels, and parser widths; use narrow boundary conversions when needed rather than changing internal semantics just to make a warning disappear.

### Commit-scoped checkers must tolerate files deleted by the commits they inspect — !1533

Merged master MR !1533 updates `check_spelling.py`, `check_tfs.py`, and `check_typed_item_calls.py` to test whether each candidate file still exists before opening it. Commit-oriented tooling can legitimately encounter a path that was deleted by the very commit range under review.

**Rule:** a checker that derives its input list from Git history must treat deletion or rename as a normal repository event. Report or skip a path that no longer exists instead of throwing an unrelated filesystem exception that hides the actual review result.

### Hash and correlation code must respect address-object lengths — !1526

Merged master MR !1526 fixes IEEE 1905 reassembly hashing that assumed both source and destination addresses were six bytes. The accepted code builds the hash input from `key->src.len`, `key->dst.len`, the fragment ID, and VLAN ID.

**Rule:** when protocol identity uses Wireshark `address` objects, derive serialization and hashing from their actual lengths. Do not silently assume Ethernet-size addresses unless the protocol contract guarantees that address family.

### Generated dissector work belongs in the generator inputs — !1550, !1512

In merged !1550, Pascal Quantin asked the contributor to add the new Kerberos SPAKE ASN.1 module to the ASN.1 CMake input and use the supported CMake target to regenerate `packet-kerberos.c`. Merged !1512 likewise integrates the GOOSE change into the ASN.1 conformance/template source and regenerates the output. These are early corroboration of the notebook's stronger later source-of-truth rule.

### Let sample captures resolve uncertain wire-layout assumptions — !1532

Merged master MR !1532, authored by John Thacker, resolves uncertainty about where Atheros padding appears relative to an 802.11s mesh control field using an actual sample capture, then places the padding adjustment before mesh-header interpretation.

**Rule:** when implementation behavior depends on an undocumented or uncertain capture-device wire-layout detail, prefer representative packet evidence over speculation and preserve the rationale in the code.

### Keep explicit selection semantics separate from unrelated global filters — !1548 and !1549

Merged !1548 removes the RTP Player's implicit rejection of packets that fail the current display filter. The player is handed an explicit set of RTP streams by the launching workflow; silently applying the global display filter again could make the requested stream appear in the player but omit its audio or waveform. !1549 instead adds an explicit "Limit to display filter" choice to the selecting VoIP/RTP dialogs.

**Rule:** when a downstream view receives an explicit object or stream selection, do not silently reapply an unrelated global filter unless that filter is part of the view's documented contract. Put the filtering decision at the layer that chooses the set.

### Stateful UI/tap work needs lifecycle and empty-capture testing — !1557, !1549

Merged !1557 uses a `QPointer` guard so the RTP Player's recap/tap-finish path does not dereference a dialog destroyed while the recap is running. Separately, merged !1549 introduced a post-merge regression where opening related dialogs without a capture file could segfault; the author fixed it in follow-up !1573.

**Rule:** for Qt/tap changes, test destruction during callbacks or retap and opening the dialog with no active capture, not only the populated happy path.

### Use validators as semantic checks, not warning suppressors — !1559

In merged !1559, Martin Mathieson noticed a `value_string` with duplicate numeric value 2 for both the 3-slot and 5-slot Bluetooth packet-size preferences. The pipeline independently emitted a conflicting-entry warning. The author checked the Bluetooth specification and corrected the 5-slot value to 3.

**Rule:** when a table/checker warning reveals contradictory protocol mappings, verify against the normative protocol source and fix the mapping; do not merely suppress the diagnostic.

### Preserve meaningful commit boundaries even when squashing development noise — !1552, !1522

Stig Bjørlykke asked !1552 to merge its prerequisite !1448 first, then squash/rebase the focused follow-on. In !1522, Dario Lombardo raised the complementary concern that indiscriminately squashing unrelated fixes can destroy useful ownership, backport, and review boundaries. The durable synthesis is contextual: clean up fixup/development history for one logical change, but keep independently meaningful or backportable changes separate.

## Per-MR accounting

- !1559 — **Merged / deep.** New Bluetooth BR/EDR FHS/LMP dissection and L2CAP reassembly. Martin Mathieson and CI caught a duplicate `value_string` mapping; corrected after checking the standard.
- !1558 — **Merged / scanned.** DHCP RFC 5192 PANA Authentication Agent option; turns option 136 into a typed IPv4-list field.
- !1557 — **Merged / deep.** RTP Player callback/destruction fix using `QPointer` plus tap-listener lifetime tracking.
- !1556 — **Merged / deep.** NR RRC/RLC/PDCP ciphering-disabled propagation; review drove separation of state structures by protocol-layer ownership.
- !1555 — **Merged / scanned.** SIP parsing fix for repeated or optional contact parameters; marked for backport in review.
- !1554 — **Merged / scanned.** RTP Streams Time-of-Day presentation option.
- !1553 — **Merged / scanned.** RTP Player time-span presentation follows Time-of-Day mode.
- !1552 — **Merged / discussion-focused.** Bluetooth LE control-procedure request/response matching; review covered prerequisite ordering, squashing/rebase, and later regression follow-up.
- !1551 — **Merged / scanned.** Manual duplicate manufacturer-table cleanup plus EditorConfig rule.
- !1550 — **Merged / deep.** Kerberos SPAKE ASN.1 support; Pascal Quantin required authoritative ASN.1/CMake generation workflow and canonical commit subject.
- !1549 — **Merged / deep.** Explicit display-filter control for voice dialogs; later no-capture crash provided lifecycle regression evidence.
- !1548 — **Merged / deep.** RTP Player stops implicitly reapplying the display filter to explicitly selected streams.
- !1547 — **Merged / discussion-focused.** PDCP-NR invalid key reporting uses UAT callback error propagation.
- !1546 — **Merged / discussion-focused.** DRB warning cleanup; Anders Broman cautioned against mechanically converting offsets to unsigned because signed APIs may carry `-1` sentinels.
- !1545 — **Merged / scanned backport.** 2021 copyright update for master-3.2.
- !1544 — **Merged / scanned backport.** Same copyright update for release-3.4.
- !1543 — **Merged / deep, very high weight.** John Thacker raises libgcrypt floor to 1.5.0 based on supported distro availability and removes obsolete fallback crypto code.
- !1542 — **Merged / scanned.** 2021 copyright update on master.
- !1541 — **Merged / scanned.** TFTP DATA/ACK packets get generated frame links to their initiating request.
- !1540 — **Merged / discussion-focused.** Generator-assisted manufacturer-table duplicate cleanup; `make-manuf.py` distinguishes exact duplicate entries.
- !1539 — **Closed/unmerged / low weight.** Draft/experimental "Except" MR with unrelated test edits; no accepted implementation precedent.
- !1538 — **Merged / discussion-focused.** NR RRC updates the Info column before a parse path that may throw so top-level classification survives the exception.
- !1537 — **Merged / scanned.** TPNCP compatibility/version-aware parsing and bounds updates; discussion mostly about squash-message presentation.
- !1536 — **Merged / scanned.** Centralized RTP decimal-place preferences with bounded accepted range.
- !1535 — **Merged / scanned.** Simplifies VoIP retap call matching to stable call number rather than constructed IDs.
- !1534 — **Merged / scanned backport.** QUIC version field represented numerically with `FT_UINT32`, big-endian decoding, and symbolic values rather than ASCII.
- !1533 — **Merged / deep.** Commit-scoped checkers now handle files that no longer exist.
- !1532 — **Merged / deep, high weight.** John Thacker uses sample-capture evidence to place Atheros padding before 802.11s mesh control.
- !1531 — **Merged / scanned.** VoIP Calls retap preserves visible call objects while rebuilding newly analyzed data.
- !1530 — **Merged / scanned.** NAS EPS replaces local equivalent TFS with shared canonical TFS, prompted by `check_tfs.py`.
- !1529 — **Merged / discussion-focused.** SOME/IP dynamic filterable parameters; Guy Harris later identified a copy/paste defect, acknowledged by the author.
- !1528 — **Merged / scanned.** Removes redundant IEEE 1905 declaration.
- !1527 — **Merged / scanned.** RTP Streams adds Start Time and Duration columns.
- !1526 — **Merged / deep.** IEEE 1905 reassembly hash stops assuming six-byte addresses and serializes actual address lengths.
- !1525 — **Merged / scanned.** Master QUIC version field semantic type correction later backported by !1534.
- !1524 — **Merged / deep.** Compiler-warning cleanup with extensive Pascal Quantin review preserving precision, constness, widths, and minimal casts.
- !1523 — **Merged / scanned.** Flow Sequence can select/deselect corresponding RTP streams; adds typed auxiliary sequence-item state and cleanup.
- !1522 — **Merged / discussion-focused.** Warning cleanup plus substantive discussion of when GitLab squash should and should not collapse commit boundaries.
- !1521 — **Merged / scanned.** Removes unused funnel typedefs.
- !1520 — **Merged / scanned.** Removes duplicate GOOSE ASN.1 `FIELD_RENAME` configuration.
- !1519 — **Merged / scanned stable backport.** VoIP graph-analysis ownership fix.
- !1518 — **Merged / scanned stable backport.** Same graph-analysis ownership fix.
- !1517 — **Merged / scanned stable backport.** VoIP retap resets call statistics rather than accumulating them.
- !1516 — **Merged / scanned stable backport.** Same VoIP retap reset fix.
- !1515 — **Merged / scanned stable backport.** DHCPv6 expert-message typo correction.
- !1514 — **Merged / scanned stable backport.** Same DHCPv6 typo correction.
- !1513 — **Merged / scanned.** Corrects DNS registered-field abbreviation typo.
- !1512 — **Merged / deep.** GOOSE S-bit/simulation validation is carried through ASN.1 configuration/template and regenerated C.
- !1511 — **Merged / discussion-focused.** Master VoIP graph-analysis ownership correction; caller owns replacing the analysis object instead of generic reset freeing it unexpectedly.
- !1510 — **Merged / scanned.** Master DHCPv6 expert-message typo correction.

## VANC notebook directive

No VANC/ST 291 packet type was encountered.
