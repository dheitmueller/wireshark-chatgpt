# Automated MR review ledger: !10663–!10712

Reviewed using GPT-5.6 Sol.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed merge requests were reviewed in this run. The already-reviewed set was reconstructed from the available `reviewed-mrs.md`, aggregate automation tracking, and per-run files under `reviewed-mrs-automation/`; selection was based on individual MR membership rather than assuming numeric ranges were complete. The historical !17571–!17620 ledger was explicitly checked and continues to contribute all 50 MRs to the reviewed set.

Status mix: **47 merged, 3 closed/unmerged drafts (!10700, !10698, !10682)**. Merged master MRs were weighted most heavily; stable backports mainly corroborate their master changes, and closed drafts were treated as contextual/negative evidence. Maintainer-authored and maintainer-reviewed guidance, especially from Guy Harris and John Thacker, received correspondingly high weight.

| MR | Outcome | Review result |
|---|---|---|
| !10712 | merged | MSYS2 build documentation refresh; documentation-specific. |
| !10711 | merged release-3.6 | Decode As explicit-none crash backport; corroborates !10709/!10732. |
| !10710 | merged release-4.0 | Decode As explicit-none crash backport; corroborates !10709/!10732. |
| !10709 | merged master | John Thacker guards Decode As preference processing when the selected handle is intentionally NULL; strong corroboration of the existing default-vs-none state rule. |
| !10708 | merged release-3.6 | TCP reused-port false-positive backport. |
| !10707 | merged release-4.0 | TCP reused-port false-positive backport. |
| !10706 | merged master | Prevents retransmission handling from also marking a TCP conversation as port reuse; protocol-specific correctness fix. |
| !10705 | merged master | Decode As Qt model exposes the default dissector through a getter and tightens member encapsulation. |
| !10704 | merged master, deep/promoted | John Thacker replaces packet-controlled direct indexing of an incomplete `value_string` with `val_to_str_const(..., "Unknown")`. Promoted to `field-value-semantics-conventions.md`. |
| !10703 | merged master | Windows GnuTLS package/dependency update. |
| !10702 | merged master | GSSAPI header removes unnecessary DCE/RPC header dependency via forward declarations. |
| !10701 | merged master | Martin Mathieson checker-driven field-width/mask cleanup across dissectors; strong corroboration of typed-item field semantics. |
| !10700 | closed draft | Empty “More item lengths” draft; down-weighted, no accepted implementation. |
| !10699 | merged master | Decode As suggestions consider all occurrences/layers of a protocol in the packet; Qt-specific behavior. |
| !10698 | closed draft | Proposed reassembly-head reference counting; unmerged, retained only as context. |
| !10697 | merged master | PostgreSQL GSS-API session-encryption support; substantial protocol feature. |
| !10696 | merged master, deep/corroborating | John Thacker snapshots MySQL prepared-statement ID per frame so random access does not read the conversation's latest value. Reinforces `dissector-state-conventions.md`. |
| !10695 | merged master | TLS tolerates buggy cleartext renegotiation after CCS; its own comments document the redissection risk of mutating coarse session state. |
| !10694 | merged release-3.6 | NetScaler bounds-hardening backport of !10692. |
| !10693 | merged release-4.0 | NetScaler bounds-hardening backport of !10692. |
| !10692 | merged master, deep/promoted | Guy Harris validates the fixed NetScaler record header within the current page before typed access. Promoted to `wire-structure-decoding-conventions.md`. |
| !10691 | merged release-4.0 | TCP OOO-state consistency backport of !10690. |
| !10690 | merged master, deep/promoted | John Thacker requires both the OOO preference and the actually initialized OOO segment list before reassembly. Promoted to `runtime-state-consistency-conventions.md`. |
| !10689 | merged master | Makes an SBAS CRC helper file-local/static. |
| !10688 | merged master | Windows libgcrypt dependency/package update. |
| !10687 | merged release-4.0 | Replaces RTPS `g_strlcpy` magic destination sizes with `sizeof`; buffer-maintenance corroboration. |
| !10686 | merged master, discussion-focused | TCP completeness for conversations pre-created by higher protocols; John Thacker drives first-pass/new-conversation reasoning and requests representative simultaneous-open captures. |
| !10685 | merged master, deep/promoted | RDP dynamic channel IDs can be reused in one connection; accepted multimap versions ID mappings by frame and looks up the mapping valid at the current frame. Promoted to `dissector-state-conventions.md`. |
| !10684 | merged master | Diameter passes request/answer context down to the SMS AVP dissector to select direction; protocol-specific context propagation. |
| !10683 | merged master | Broad checker-driven field-width corrections; corroborates typed-item/value-domain conventions. |
| !10682 | closed draft | Proposed creating missing TCP OOO lists after a preference/state mismatch; superseded by merged !10690's safer realized-state check. |
| !10681 | merged release-3.6 | BLF success-only output-parameter logging fix backport of !10679. |
| !10680 | merged release-4.0 | BLF success-only output-parameter logging fix backport of !10679. |
| !10679 | merged master, deep/promoted | Guy Harris removes error-path logging of an output parameter not guaranteed initialized on failure. Promoted to `error-path-contract-conventions.md`. |
| !10678 | merged release-3.6 | BLF zlib cleanup-on-error backport. |
| !10677 | merged release-4.0 | BLF zlib cleanup-on-error backport. |
| !10676 | merged master | Guy Harris calls `inflateEnd()` on the zlib failure path; reinforces cleanup ownership rules. |
| !10675 | merged release-3.6 | BLF read-error allocation/leak backport. |
| !10674 | merged release-4.0 | BLF read-error allocation/leak backport. |
| !10673 | merged master | Guy Harris delays allocation until after validation and frees the buffer when a read fails; strong cleanup corroboration. |
| !10672 | merged release-3.6 | BLF error-semantics backport of !10670. |
| !10671 | merged release-4.0 | BLF error-semantics backport of !10670. |
| !10670 | merged master, deep/promoted | Guy Harris prevents nested compressed-data truncation from being misinterpreted as normal EOF. Promoted to `error-path-contract-conventions.md`, while preserving the notebook's newer BAD_FILE-vs-INTERNAL taxonomy. |
| !10669 | merged release-4.0 | BLF app-text NUL-termination backport. |
| !10668 | merged release-3.6 | VMS parser tracks “length seen” separately from numeric length so zero is not overloaded as absence; corroborates explicit-state/sentinel guidance. |
| !10667 | merged release-3.6 | MS-MMS uses the NUL-terminated-string formatting helper rather than carrying a redundant manual length. |
| !10666 | merged release-4.0 | Same MS-MMS string-helper correction. |
| !10665 | merged master, deep/corroborating | John Thacker explicitly separates MySQL conversation “latest first-pass state” from per-frame state used for random-access redissection. |
| !10664 | merged master | Guy Harris allocates BLF app text with one extra byte and appends the terminator explicitly instead of assuming file data is NUL-terminated. |
| !10663 | merged master, deep/promoted | John Thacker stores persistent request bytes instead of a long-lived real-data tvbuff and creates a child tvbuff per consuming packet. Promoted to `tvbuff-persistent-state-lifetime-conventions.md`. |

## Durable findings promoted

- !10704: packet data must not directly index a `value_string`; use mapping helpers with defined unknown behavior.
- !10692 (Guy Harris): validate a bounded page/record contains the fixed header before forming/using its typed view.
- !10685: reusable protocol identifiers need temporally versioned state so random-access lookup sees the mapping valid at that packet.
- !10663: persist backing bytes when they need file scope, but keep temporary tvbuff wrappers tied to packet/parent lifetime.
- !10690: current configuration and realized state are separate facts; optional processing requires both or a defined rebuild transition.
- !10679 (Guy Harris): do not read success-only output parameters on a failure path.
- !10670 (Guy Harris): preserve failure semantics across helper boundaries so nested truncation cannot silently become outer EOF; current BAD_FILE-vs-INTERNAL guidance remains authoritative.

## Strong corroboration retained without duplicate rules

- !10665 and !10696 strengthen the existing first-pass/per-frame MySQL snapshot guidance.
- !10709 plus stable !10710/!10711 reinforce that Decode As “none” is an explicit state, not a reset/default sentinel.
- !10701 and !10683 reinforce checker-driven field type/width/mask correctness.
- !10676 and !10673, plus their stable backports, reinforce resource cleanup on every read/decompression error path.
- !10668 reinforces the existing rule not to overload a legitimate numeric zero with “not present/not seen.”
- !10686 is a good review/testing exemplar: John Thacker reasoned through first-pass state and an unusual simultaneous-open case, then asked for focused captures rather than generalizing without evidence.

## Exact reviewed MR set

!10712 !10711 !10710 !10709 !10708 !10707 !10706 !10705 !10704 !10703 !10702 !10701 !10700 !10699 !10698 !10697 !10696 !10695 !10694 !10693 !10692 !10691 !10690 !10689 !10688 !10687 !10686 !10685 !10684 !10683 !10682 !10681 !10680 !10679 !10678 !10677 !10676 !10675 !10674 !10673 !10672 !10671 !10670 !10669 !10668 !10667 !10666 !10665 !10664 !10663
