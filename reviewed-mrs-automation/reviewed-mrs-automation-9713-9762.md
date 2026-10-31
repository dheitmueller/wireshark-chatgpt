# Automated Wireshark MR review — !9762 through !9713

Review date: 2026-10-30
Corpus repository: `dheitmueller/wireshark-corpus-mrs`
**Corpus commit reviewed:** `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `b3f8caad8da02894c7b7c0d992db2bd244a2f9dc`
Direction: highest-numbered previously unreviewed MR first.

**Exactly 50 distinct records in descending order: 47 merged, one open (!9742), and two closed (!9721, !9718).**

## Selection and duplicate review controls

Checked the current corpus and notebook heads, inventoried the 350 per-run/other tracking entries under `reviewed-mrs-automation/`, and consulted the aggregate `reviewed-mrs.md`, supplemental `reviewed-mrs-automation.md`, adjacent exact-membership ledger !9812–!9763, plus exceptional/noncontiguous/backfill/gap ledgers. The target entries were not marked already reviewed in the checked tracking. Numeric filename ranges alone were not assumed to prove complete review. Each of the 50 specific corpus JSON files was fetched, and retained discussions and final diffs were inspected; MR !9744 has a missing large-file diff in the corpus and therefore was reviewed through discussion and available metadata. Merged outcomes and implemented maintainer review weighed more strongly than closed/superseded proposals.

The historical `reviewed-mrs-automation-17571-17620.md` was independently checked for **50 distinct explicit membership rows covering !17571–!17620** and is unchanged.

## Exact reviewed-MR membership

| MR | Corpus outcome | Depth | Review note |
| --- | --- | --- | --- |
| !9762 | merged | Discussion-focused | CoAP OCF version parsing; Stig Bjørlykke asked that file-local masks/macros be kept in packet-coap.c. |
| !9761 | merged | Scanned | NAS-5GS UE OS ID field registration and dissection correction. |
| !9760 | merged | Scanned | F5 r5000 model-name typo, release-3.6 backport of !9756. |
| !9759 | merged | Scanned | F5 r5000 model-name typo, release-4.0 backport of !9756. |
| !9758 | merged | Deep | Guy Harris: TLS/DTLS 1.3 ClientHello legacy-version misuse gets expert warning, without abandoning protocol recognition. |
| !9757 | merged | Deep | RTPS ACKNACK/GAP/HEARTBEAT topic name in Info column from discovery and packet-scoped protocol metadata. |
| !9756 | merged | Scanned | Master F5 r5000 model-name typo correction, two backports. |
| !9755 | merged | Scanned | Gerald Combs adds ChmodBPF macOS upgrade/reinstall release-note guidance. |
| !9754 | merged | Scanned | Gerald Combs removes hard-coded macOS auxiliary package versions from installer XML. |
| !9753 | merged | Deep | Martin Mathieson passes encoding/endian into custom pcapng option dissector callback. |
| !9752 | merged | Deep | John Thacker CBOR skip-on-error must not roll back offset, avoiding infinite loop/memory exhaustion. |
| !9751 | merged | Deep | USBLL STALL resets transfer keys, PID and reassembly state, including split endpoint direction. |
| !9750 | merged | Scanned | F5 rSeries IDs release-3.6 backport of !9726. |
| !9749 | merged | Scanned | F5 rSeries IDs release-4.0 backport of !9726. |
| !9748 | merged | Scanned | RSVP P2MP ID unsigned display and name fix release-3.6 backport. |
| !9747 | merged | Scanned | RSVP P2MP ID unsigned display and name fix release-4.0 backport. |
| !9746 | merged | Scanned | Qt minimap overlay fencepost fix release-3.6 backport of !9740. |
| !9745 | merged | Scanned | Qt minimap overlay fencepost fix release-4.0 backport of !9740. |
| !9744 | merged | Discussion-focused | Gerald Combs explains libc index shadow warning; CI flags VALS64 missing BASE_VAL64_STRING; large-file diff omitted. |
| !9743 | merged | Scanned | Cisco metadata tree item highlight spans eight rather than six bytes. |
| !9742 | opened | Discussion-focused | Open draft CoAP Q-Block refactor split from !9731; code ordering reviewed but feature not accepted. |
| !9741 | merged | Deep | John Thacker removes unnecessary packet-list resize causing Qt scrollbar/minimap disappearance. |
| !9740 | merged | Scanned | Qt minimap final-row rendering fencepost correction on master. |
| !9739 | merged | Scanned | Remove duplicate RTPS entity-kind value_string mappings that produced conflicting-label warnings. |
| !9738 | merged | Scanned | Master RSVP P2MP ID label and unsigned format correction. |
| !9737 | merged | Deep | John Thacker invalidates colorized row cache when coloring-rule generation changes. |
| !9736 | merged | Scanned | macOS uninstall package-receipt cleanup release-3.6 backport of !9730. |
| !9735 | merged | Scanned | macOS uninstall package-receipt cleanup release-4.0 backport of !9730. |
| !9734 | merged | Scanned | macOS codesign org.wireshark. prefix, release-3.6 backport. |
| !9733 | merged | Scanned | macOS codesign org.wireshark. prefix, release-4.0 backport. |
| !9732 | merged | Deep | Gilbert Ramirez review: replace ambiguous Boolean smpp.command_type with generated smpp.request/response fields. |
| !9731 | merged | Deep | Stig Bjørlykke requests CoAP regression fix be split from slower Q-Block feature; fix merged, enhancement moved to !9742. |
| !9730 | merged | Deep | Gerald Combs forgets both macOS helper receipts after Guy Harris and Peter Wu review; stable backports. |
| !9729 | merged | Scanned | Jaap Keuter refreshes extcap argument-type documentation on master. |
| !9728 | merged | Scanned | Extcap argument-type documentation release-4.0 backport. |
| !9727 | merged | Scanned | TShark pcapng packet-option block refcount fix release-4.0 backport of !9716. |
| !9726 | merged | Scanned | Master F5 rSeries hardware identifier update with two stable backports. |
| !9725 | merged | Discussion-focused | Documents WTAP_OPEN_* outcomes, open_info_base and file-extension registration, reader-state cleanup. |
| !9724 | merged | Discussion-focused | Alexis La Goutte requests sample pcap for BGP software-version capability; author attaches it. |
| !9723 | merged | Scanned | Correct wsutil Buffer non-macro function gsize types and export declarations. |
| !9722 | merged | Deep | RTPS PL_CDR decode after decompression uses data_holder_tvb rather than original source tvb. |
| !9721 | closed | Scanned | Closed RTPS decompression draft with no accepted patch; !9722 supplies merged successor. |
| !9720 | merged | Deep | John Thacker replaces temporary PROFINET fixed-size C strings with proto_tree_add_item ENC_UTF_8. |
| !9719 | merged | Deep | João Valverde moves compile/runtime version reporting out of UI to wsutil, fixes consumers and linkage. |
| !9718 | closed | Discussion-focused | Closed direct-GCRYPT-link workaround rejected by João Valverde: RHEL7 transitive dependency ABI mismatch. |
| !9717 | merged | Deep | João Valverde moves exit codes out of UI into shared include/ws_exit_codes.h, with WS_EXIT_* naming. |
| !9716 | merged | Deep | John Thacker refs wtap option block across epan dissection/reset to preserve packet options on write. |
| !9715 | merged | Scanned | João Valverde updates ELF relocation with static wsutil for privileged helper and $ORIGIN caveat. |
| !9714 | merged | Scanned | João Valverde adds missing wiretap link dependency to UI for MSYS2. |
| !9713 | merged | Deep | João Valverde notes dissector headers historically over-exported; Martin Mathieson recommends static/internal by default. |

## Durable findings and evidence weighting

- !9752: John Thacker's CBOR parser must advance rather than roll back on errors.
- !9751: USB STALL ends the previous transfer/reassembly epoch.
- !9753: Martin Mathieson's pcapng option callbacks require encoding from their containing block.
- !9758: Guy Harris's TLS/DTLS ClientHello conformance check distinguishes warning from recognition.
- !9732: Gilbert Ramirez's Boolean naming guidance becomes generated `smpp.request`/`smpp.response` filter fields.
- !9744: Gerald Combs on portability of `index`; `VALS64` checker and `BASE_VAL64_STRING` consistency.
- !9731/!9742: Stig Bjørlykke separates a merged regression fix from draft feature work.
- !9718: João Valverde rejects a dependency-link workaround masking a transitive ABI problem.
- !9719/!9717/!9713: shared version/exit contracts live below UI; dissector exports need demonstrated consumers.
- !9716/!9727: preserve Wiretap packet-option block references across dissection and output.
- !9737/!9741/!9740: Qt cache versions, scrollbar resizing and minimap row boundaries.
- !9720: UTF-8-aware tvbuff string extraction; !9722: transformed RTPS data needs the transformed tvbuff.
- !9730: macOS uninstall must forget all relevant package receipts, following Guy Harris and Peter Wu.
- !9725: Wiretap reader open outcomes, extension registration and private cleanup contracts.

Closed !9718 and !9721 are not accepted implementations; open draft !9742 is not yet approved behavior. Routine stable backports and maintenance fixes were recorded without creating redundant rules.

**Next expected candidate: !9712**, subject to fresh corpus and exact-membership checks.
