# Findings for !9713–!9762

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs, descending from !9762 through !9713. Forty-seven were merged; !9742 was open, while !9721 and !9718 were closed/unmerged and were down-weighted.

## Strong durable findings

- **!9752 — parser progress, merged, John Thacker authored/merged.** Shared CBOR skip logic rewound the offset to the start of an erroneous chunk, contradicting the skip helper's recovery contract and permitting repeated processing, memory exhaustion, or an infinite loop for malformed large lists. Added to `dissector-consumption-boundary-conventions.md`.
- **!9732 — field semantics, merged, Gilbert Ramirez review.** A proposed Boolean `smpp.command_type` was ambiguous in both positive and negated display filters. The accepted design exposes generated `smpp.request` / `smpp.response` fields instead. Added to `field-value-semantics-conventions.md`.
- **!9731 — submission scope, merged, Stig Bjørlykke review.** A simple regression fix and slower Q-Block refactor were split so the bug fix could be reviewed and merged independently; the feature work moved to draft !9742. Added to `submission-backport-scope-conventions.md`.
- **!9720 — typed string APIs, merged, John Thacker authored/merged.** PROFINET replaces manual allocation/copy/NUL termination with direct `proto_tree_add_item(..., ENC_UTF_8)`, centralizing encoding and trailing-NUL validation. Added to `text-encoding-conventions.md`.
- **!9716 / !9727 — ownership lifetime, merged, John Thacker authored/merged.** tshark must take its own `wtap_block_ref()` before dissection/reset can unreference the record block that later output still needs. Added to `architecture.md`.
- **!9713 — header/API boundary, merged.** Martin Mathieson reported 274 symbols declared in dissector headers but not referenced by other modules and argued that most should be static/deleted unless a real cross-module/private-plugin contract exists; João Valverde agreed. This strongly corroborates the notebook's existing “genuine cross-file contracts only” header rule.

## High-authority corroborating evidence

- **!9758 — merged, Guy Harris-authored.** TLS/DTLS ClientHello legacy-version values that violate the 1.3 specification remain recognizable/dissectable but receive expert warnings. Strong corroboration that tolerated protocol nonconformance should be surfaced diagnostically rather than causing misclassification or hard rejection.
- **!9744 — merged.** Gerald Combs explains that `index` is still a C-library symbol on some supported systems even though deprecated, so a variable name can trigger legitimate cross-platform shadow warnings that are not visible on every developer host. The same MR also demonstrates checker enforcement for 64-bit `VALS64` display semantics.
- **!9730 — merged, Guy Harris review.** When uninstall logic forgets one package receipt, audit sibling packages/components that implement the same lifecycle operation; Guy specifically asked that path-helper uninstallation be handled too.
- **!9724 — merged.** Alexis La Goutte requested a representative BGP capture and the contributor attached one before merge, corroborating the established sample-capture expectation for protocol changes.
- **!9719 / !9717 — merged.** Generic version information and exit-code contracts move out of the UI layer into common support/include ownership, reinforcing the common-layer rule already established by nearby !9780.
- **!9715 / !9714 — merged.** The accepted relocatable-install design accounts for dumpcap's elevated-privilege loader constraints and then fixes an MSYS2 linkage regression, reinforcing validation across supported build environments.
- **!9718 — closed/down-weighted.** João Valverde rejected adding an apparently unrelated transitive crypto library merely to paper over one RHEL7 local dependency combination. Diagnose the actual dependency mismatch rather than teaching the project to link unrelated libraries globally.

## Other reviewed MRs

- !9762 CoAP OCF semantic-version option decoding; Stig Bjørlykke kept implementation-only defines file-local.
- !9761 NAS-5GS UE OS ID decoding corrected to the specified UUID-sized value.
- !9760, !9759, !9756 F5 platform-name typo correction across master/stable.
- !9757 RTPS topic-name context propagated to additional submessages.
- !9755, !9754 macOS ChmodBPF/package release maintenance.
- !9753 pcapng local-option callback gains encoding context needed by extension dissectors.
- !9751 USBLL STALL state stops reassembly across failed endpoint transactions.
- !9750, !9749, !9726 F5 platform identifier updates across branches.
- !9748, !9747, !9738 RSVP P2MP summary fixes and stable backports.
- !9746, !9745, !9740 packet-list minimap fencepost correction across branches.
- !9743 Cisco metadata protocol-item highlight length corrected.
- !9742 open draft Q-Block follow-up; down-weighted.
- !9741 Qt packet-list resize path no longer unnecessarily hides scrollbar/minimap.
- !9739 RTPS conflicting duplicate value-string entry removed after runtime warnings.
- !9737 packet-list colorization now tracks coloring-rule version.
- !9736, !9735 macOS package-forget stable backports.
- !9734, !9733 macOS signature-identifier stable backports.
- !9732 SMPP request/response field semantics.
- !9731 focused CoAP regression fix after scope split.
- !9730 macOS package-forget master fix with sibling-component audit.
- !9729, !9728 extcap argument-type documentation synchronized.
- !9727 release-4.0 backport of tshark record-block lifetime fix.
- !9725 wiretap open-routine comments use named `WTAP_OPEN_*` semantics.
- !9724 BGP Software Version Capability plus representative capture.
- !9723 `wsutil/buffer.h` alternate non-macro function signatures repaired.
- !9722 RTPS compressed-data dissection corrected to use the decompressed TVBuff.
- !9721 closed draft precursor; merged !9722 is authoritative.
- !9720 PROFINET typed TVBuff string handling.
- !9719 version-information ownership moved from UI to wsutil.
- !9718 closed dependency-workaround proposal; down-weighted.
- !9717 common exit-code definitions moved out of UI.
- !9716 tshark record-block ownership fix.
- !9715 Linux relocatable-install take 2 handles privileged dumpcap specially.
- !9714 MSYS2 linkage correction.
- !9713 dissector-header scope guidance and static-analysis evidence.

The exact per-MR membership is authoritative in `reviewed-mrs-automation-9713-9762.md`.
