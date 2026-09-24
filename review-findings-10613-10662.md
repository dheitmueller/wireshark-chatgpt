# Wireshark review findings: !10613–!10662

This file records durable conventions extracted from the batch. Current upstream source remains authoritative.

## Capture-file input boundary

Merged !10618 began as an extcap for EGNOS Message Server files. Guy Harris pointed out that the main use case was reading completed files, which belongs in libwiretap. The author converted the change to a Wiretap reader before merge.

Rule: use Wiretap for native offline capture-file formats; use extcap for external acquisition or genuinely live external sources. If an extcap primarily opens a filename, review whether it should be a Wiretap reader.

## Multi-source pcapng interface identity

Merged master !10645, with release backports !10661 and !10662, stops creating fake unknown-linktype IDBs for pcapng inputs. John Thacker preserves source IDBs and remaps source-local interface IDs into the combined output namespace. Guy Harris emphasized that separate pcapng inputs can both use interface ID 0 and therefore require remapping.

Rule: preserve authoritative source IDBs and explicitly map source-local interface identity when combining inputs. Synthesize IDBs only for source paths that do not supply them.

## Shared reassembly-result lifetime

Merged !10641 adds reference counting to reassembled fragment heads because one completed result can be stored under multiple keys and keys can be replaced.

Rule: table keys and reassembled-object ownership are separate concerns. When several mappings can refer to one result, treat mappings as references and free the result only after the last reference is gone.

## Typed field value construction

Merged !10626 fixes SIP header/body fields. Shortening a protocol item's displayed byte range after creating an FT_STRING does not rebuild the already-extracted string value; exporters and filters can still see the old oversized value.

Rule: determine a string field's semantic byte extent before constructing its typed value. !10657 independently reinforces that converted text length is distinct from original packet byte length.

## Metadata edits and redissection

Merged master !10620 with stable backports !10623 and !10624 makes time shifting trigger redissection because fields such as relative TCP/SMB time and SRT state depend on file-scoped first-pass data.

Rule: edits to packet metadata that feed file-scoped analysis require normal invalidation/redissection, not only a UI refresh.

## Wiretap errors and untrusted lengths

Guy Harris's merged !10625 changes BLF container loading from debug-only failure to structured Wiretap errors with err/err_info; !10628 and !10630 backport it. Malformed file state should normally be WTAP_ERR_BAD_FILE, while true Wireshark invariants belong under WTAP_ERR_INTERNAL.

Guy's merged !10617, with !10619 and !10621 backports, validates CAN/CAN-FD file-declared lengths before bounded copies and reports overlength records as WTAP_ERR_BAD_FILE. His merged !10656/!10659 VMS fix likewise distinguishes a missing required length field from a legitimate numeric zero.

Rule: nested Wiretap helpers must return useful error categories and context, and file-declared sizes must be validated before bounded copies.

## Corroborating review evidence

!10637 broadens typed-item length checking and !10650 fixes concrete findings; later notebook guidance remains authoritative that such checks must model each proto-tree API variant semantically. !10655 reinforces sizing bounded string copies from actual destination capacity. !10614 verifies a complete fixed SAP HANA DB header before tcp_dissect_pdus. !10651 fixes repeated-element bounds relative to the element start.

Closed !10660 and !10633 were superseded by merged equivalents and are down-weighted. The corpus artifact for closed !10629 is empty; the upstream GitLab record shows an accidental wrong-target backport with 881 changes that Guy Harris immediately closed, so it contributes no accepted convention.
