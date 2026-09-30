# Wireshark MR review findings 3811-3860

Date: 2026-09-30
Model: GPT-5.6 Sol
Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Selection was checked by exact MR membership against root reviewed-mrs.md, the supplemental aggregate ledger, the predecessor exact ledger, and the available irregular gap/backfill/noncontiguous/exact-list/reconciliation tracking. No MR in this batch was already recorded. The historical 17571-17620 batch was also rechecked as 50 unique entries.

Outcomes: 48 merged and 2 closed (!3859 and !3833). Closed work is used only as lower-weight review/supersession evidence.

| MR | State | Author | Finding |
| ---: | --- | --- | --- |
| !3860 | merged | Guy Harris | release-3.4 backport of !3858; editcap -T updates interface link-type metadata. |
| !3859 | closed | Guy Harris | earlier release-3.4 backport attempt; Guy said it needed changes for 3.4; superseded by !3860. |
| !3858 | merged | Guy Harris | master origin: editcap -T copies IDBs, changes copied wtap_encap values, writes/unrefs them, and preserves retained originals for later split files. |
| !3857 | merged | John Thacker | corrects editcap split-output filename documentation. |
| !3856 | merged | Guy Harris | uses PACK_FLAGS_DIRECTION() instead of open-coded masking and narrows the ETW flag variable scope. |
| !3855 | merged | David Perry | RTPS moves native per-packet participant GUID state from pinfo->private_table to packet-scoped proto-data. |
| !3854 | merged | David Perry | removes Section N prose from capinfos table output; Guy suggested a schema that separates file-wide and per-section data. |
| !3853 | merged | David Perry | capinfos machine modes emit canonical Wiretap file-type/encapsulation names; tests expect identifiers such as ether and rawip4. |
| !3852 | merged | Gerald Combs | automatic release-3.2 data/translation refresh; no durable engineering lesson. |
| !3851 | merged | Gerald Combs | automatic release-3.4 data/translation refresh; no durable engineering lesson. |
| !3850 | merged | Gerald Combs | automatic master data/translation refresh; no durable engineering lesson. |
| !3849 | merged | Martin Mathieson | dissector/document spelling cleanup and word-list updates. |
| !3848 | merged | Martin Mathieson | extends typed-item checking to ptvcursor APIs and fixes NFAPI registered-width mismatches exposed by that coverage. |
| !3847 | merged | Martin Mathieson | O-RAN ext11 disableBFs/orphaned-PRB correction; protocol-specific. |
| !3846 | merged | Dr. Lars Völker | adds AUTOSAR FlexRay TP frame formats to ISO15765. |
| !3845 | merged | Arkady Gilinsky | review establishes dissector-local helper prefixes, rejects unnecessary inline, and records local squash plus meaningful commit-message guidance. |
| !3844 | merged | John Thacker | release-3.4 backport of captype documentation fixes. |
| !3843 | merged | Dr. Lars Völker | ISO15765 cleanup and frame-length display correction. |
| !3842 | merged | Joakim Andersson | release-3.4 backport of !3841 Bluetooth sync-info offset fix. |
| !3841 | merged | Joakim Andersson | fixes use of a subset-TVBuff-relative returned offset by adding the parent TVBuff base offset. |
| !3840 | merged | John Thacker | captype documentation name/reference corrections. |
| !3839 | merged | Jörg Mayer | adds initial EDP physical Linkinfo dissection. |
| !3838 | merged | gtker | large WoW areas/maps/movement expansion and ptvcursor refactor; little reusable review guidance. |
| !3837 | merged | Jaap Keuter | adds NSH Next Protocol None; accepted successor to closed !3833. |
| !3836 | merged | Jaap Keuter | generator emits a trailing newline in generated source. |
| !3835 | merged | Martin Mathieson | O-RAN U-plane/C-plane section fixes. |
| !3834 | merged | Dr. Lars Völker | first FlexRay TP support in ISO15765; precursor to !3846. |
| !3833 | closed | atul358 | NSH None proposal superseded by merged !3837. |
| !3832 | merged | David Perry | introduces caller-side wtap_rec_reset() to release record blocks; useful history, later superseded by central ownership in !4042. |
| !3831 | merged | Dr. Lars Völker | BLF cleanup and start-time handling. |
| !3830 | merged | Joakim Karlsson | RADIUS 3GPP 5G AVP dictionary expansion; no durable review convention extracted. |
| !3829 | merged | Uli Heilmeier | follow-up fixes basename extraction in validate-clang-check.sh. |
| !3828 | merged | Dario Lombardo | fixes optional-library builds by gating Zlib-dependent state and marking a parameter unused when Kerberos is absent. |
| !3827 | merged | Uli Heilmeier | release-3.4 clang-check shell portability/target filtering; later !4273 is stronger build-matrix precedent. |
| !3826 | merged | Joakim Karlsson | NAS 5GS UE policy section-management result support. |
| !3825 | merged | Gerald Combs | Wiretap warning cleanup/staticization; coordinates with !3831. |
| !3824 | merged | Gerald Combs | gives generated PDF/EPUB guides descriptive filenames. |
| !3823 | merged | Jaap Keuter | Wiretap header documentation/style cleanup. |
| !3822 | merged | Uli Heilmeier | release-3.4 iLBC dependency/API compatibility fix, part 2. |
| !3821 | merged | Uli Heilmeier | release-3.4 iLBC clang-check exclusion for an optional unbuilt source, part 1. |
| !3820 | merged | Gerald Combs | makes Linux build/test CI jobs explicitly require Docker-tagged runners. |
| !3819 | merged | Joakim Karlsson | PFCP update to 3GPP TS 29.244 V17.1.0. |
| !3818 | merged | Dylan Ulis | CIP Safety CRC-S5 refactor removes duplicate code and reports checksum failure at the correct semantic level. |
| !3817 | merged | Dylan Ulis | adds hidden generic cip.connid fields so one filter matches connection IDs across CIP/ENIP; fixes unsigned sequence display. |
| !3816 | merged | Martin Mathieson | documentation spelling corrections. |
| !3815 | merged | Martin Mathieson | makes dissector-only variables file-static. |
| !3814 | merged | Arkady Gilinsky | OAMPDU queue-object parsing; predecessor to review-heavy !3845. |
| !3813 | merged | Piotr Winiarczyk | Bluetooth Mesh scheduler/time opcodes; Gerald advises _U_ on intentionally unused callback parameters. |
| !3812 | merged | Joakim Karlsson | PFCP update to 3GPP TS 29.244 V17.0.0. |
| !3811 | merged | Dr. Lars Völker | adds CAN bus identity to SocketCAN/TECMP/Signal-PDU state and keys mappings by CAN ID plus bus ID with an explicit bus-zero wildcard. |

## Strongest durable conclusions

- !3858 (Guy Harris): capture transformations must keep packet/file/interface link-layer metadata consistent. editcap -T transforms copied IDBs while preserving retained originals for later split outputs; !3860 is the stable backport.
- !3855: native packet state belongs in scoped proto-data, not pinfo->private_table. The RTPS change makes both ownership and packet lifetime explicit.
- !3853 and !3854: machine output needs canonical identifiers and an explicit structural schema. Canonical Wiretap names replace human descriptions; Guy's review on capinfos highlights file-wide versus per-section cardinality.
- !3848 (Martin Mathieson): static field-contract checking should cover equivalent proto-tree API families. Extending the checker to ptvcursor immediately exposed real field-width mismatches.
- !3845: local helper naming and submission history are review interfaces. Anders Broman required a dissector-specific prefix rather than proto_; Pascal Quantin reiterated local squashing with a meaningful commit message because automation can disturb GitLab auto-squash state.
- !3841: offsets returned while operating on a subset TVB are subset-relative unless the API says otherwise; parents indexing the original TVB must translate the coordinate system.
- !3811: correlation keys must include the context that scopes a reusable numeric identifier. CAN ID alone is insufficient when one trace contains multiple buses.

## Historical or corroborating evidence

- !3832 is useful record-lifecycle history, but later reviewed !4042 is stronger current architectural precedent because common Wiretap code centralizes record-block ownership.
- !3827/!3829 are early checker target/platform evidence, but later Guy-authored !4273 is the stronger rule that source checkers must model the actual build/target matrix.
- !3845 independently corroborates the later notebook squash guidance from !5816.
