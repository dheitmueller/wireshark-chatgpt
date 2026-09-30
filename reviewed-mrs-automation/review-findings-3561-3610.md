# Wireshark MR review findings: !3561–!3610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously-unreviewed merge requests in descending order. Merged master work is treated as stronger implementation precedent than backports, closed drafts, duplicates, or superseded submissions. Direct maintainer review is weighted according to authority; Guy Harris-authored guidance is called out explicitly.

| MR | Outcome / depth | Review finding |
|---|---|---|
| !3610 | merged / backport | OSPF SRLB/SRMS Preference TLV type corrections from RFC 8665; release-3.4 corroboration, no distinct new review convention. |
| !3609 | merged / backport | master-3.2 backport of the GTPv2 eNodeB-ID six-byte length fix; corroborates !3585. |
| !3608 | merged / backport | release-3.4 backport of the GTPv2 eNodeB-ID six-byte length fix; corroborates !3585. |
| !3607 | merged / backport | master-3.2 backport of the RSL eMLPP mask correction; corroborates !3596. |
| !3606 | merged / backport | release-3.4 backport of the RSL eMLPP mask correction; corroborates !3596. |
| !3605 | merged / scanned | wslog maps both NULL and empty logging domains to the same '(none)' representation; narrow logging cleanup. |
| !3604 | merged / scanned | Removes a duplicate make-version.pl -f option registration; tooling cleanup. |
| !3603 | merged / deep historical | CMake made GLib usage private on epan, then external plugin builds were reported broken; Gerald Combs later points to !3891 as the fix. Strong negative evidence that transitive include/link requirements used by external plugins are part of the exported target contract. |
| !3602 | merged / deep | João Valverde removes wmem's wsutil dependency to keep wmem independently reusable and avoid circular/duplicate utility dependencies. Guy Harris explicitly asks whether the long-term intent is to let lower layers such as Wiretap use it; João confirms. Historical antecedent to the later wmem layering work. |
| !3601 | merged / scanned | Renames generic version.h to vcs_version.h and updates generation/build consumers; Gerald Combs confirms release tooling uses make-version.pl, so the rename does not disrupt release mechanics. |
| !3600 | merged / scanned | Uses quoted include form for an internal wmem header; consistency cleanup. |
| !3599 | merged / scanned | wslog comments/copyright cleanup; no cross-cutting rule. |
| !3598 | closed / down-weighted | Large Thrift reassembly/subdissection draft. Anders Broman warns that one huge MR and unrelated additions make review harder and suggests sequencing the compact-protocol work separately. Closed in favor of a later complete implementation. |
| !3597 | merged / scanned | Adds UAT-based diagnostic-address name resolution to DoIP; protocol feature with no substantive review convention. |
| !3596 | merged / master | Corrects RSL eMLPP Priority mask from 0x05 to 0x07 according to 3GPP 48.058; master origin for !3606/!3607. |
| !3595 | merged / backport | ASTERIX VY label typo correction on release-3.4; routine. |
| !3594 | merged / backport | ASTERIX item-number label correction on release-3.4; routine. |
| !3593 | merged / scanned | Adds UAT-based TECMP Channel-ID name resolution; no substantive review discussion. |
| !3592 | merged / Guy Harris | Guy Harris comment cleanup uses the exact owning object name ('rec') rather than stale wtap_rec/phdr terminology; narrow but authoritative naming hygiene. |
| !3591 | merged / deep | Martin Mathieson expands typed-item mask checks and fixes both cosmetic mask notation and a real RTCP registration/type problem. Historical evidence that checker findings range from warning-level shape/readability issues to semantic field-contract bugs. |
| !3590 | merged / Guy Harris | Guy Harris changes 'edited' to 'modified' because modifications are not necessarily performed by a user/editor. Reinforces state-oriented naming. |
| !3589 | merged / architectural corroboration | Reuses the more complete shared 3GPP ULI decoder for RADIUS/Diameter instead of maintaining a second divergent parser for the same wire structure. |
| !3588 | merged / Guy Harris | Guy Harris systematically renames 'user/changed' packet-block state to 'modified'; names should describe observable state rather than presume who or what caused it. |
| !3587 | merged / discussion-focused | Successful MySQL session-track resubmission with a sample pcap. Anders Broman documents amend/rebase workflow; Gerald Combs rebases via GitLab so CI can run after contributor-account pipeline restrictions. |
| !3586 | closed / down-weighted | TNS long-string work remained unmerged and was later superseded by !26411. Gerald Combs flags hostile-input loop bounds and a missing default case; later successor adds expert diagnostics and hostile-capture tests. |
| !3585 | merged / master | Corrects GTPv2 (extended) eNodeB-ID subtree length to the six bytes specified by 3GPP TS 29.274; master origin for !3608/!3609. |
| !3584 | merged / generated-code corroboration | LPP 16.5.0 update changes ASN.1 sources/templates and regenerated dissector output together; reinforces generator source-of-truth practice. |
| !3583 | merged / generated-code corroboration | NR RRC 16.5.0 update changes ASN.1/config/template inputs and regenerated output coherently. |
| !3582 | merged / generated-code corroboration | LTE RRC 16.5.0 update changes authoritative ASN.1/config/template inputs and regenerated output together. |
| !3581 | merged / scanned | Win32 string-length Coverity fix checks std::wstring state directly; narrow correctness cleanup. |
| !3580 | merged / scanned | NGAP transparent-container direction/radio-mode correction; protocol-specific state/endpoint fix. |
| !3579 | closed / duplicate | Redundant JSON self-assignment cleanup closed as duplicate immediately before merged !3578; no independent implementation weight. |
| !3578 | merged / scanned | Removes a self-assignment that Clang treats as a fatal warning; straightforward compiler-hygiene fix. |
| !3577 | merged / checker-driven | O-RAN field widths are narrowed to the bytes that actually encode the fields and related masks/value names are cleaned up; concrete semantic follow-through from typed-item mask/width checking. |
| !3576 | merged / deep | Martin Mathieson adds suspicious-mask-width and odd-hex-digit checks, explicitly noting that a wider mask can be intentional for aligned related flags. Strong historical evidence that these are heuristics requiring semantic review, not automatic errors. |
| !3575 | closed / superseded | Initial MySQL session-track MR used the fork's master branch and could not get CI; Alexis La Goutte asks for a non-master source branch. Superseded by merged !3587. |
| !3574 | merged / scanned | Dissector spelling/filter-name corrections and word-list updates; routine cleanup. |
| !3573 | merged / scanned | Win32 Coverity fixes use std::wstring ownership and remove dead code/resource leak risk; platform-local cleanup. |
| !3572 | merged / scanned | Factors common O-RAN beamforming compression helpers; local maintainability refactor. |
| !3571 | merged / platform evidence | Reverts Windows CET/EHCONT hardening because the version-gated flags caused MSVC internal compiler/linker failures during incremental builds. Supported flag availability alone is not enough; real build workflows must be exercised. |
| !3570 | merged / deep | Kerberos PAC ticket-signature support receives substantive review from Isaac Boukris and Anders Broman. Unix compilation exposed signedness/const issues; Windows exposed missing exported Kerberos functions. The accepted change adds configure-time function checks and guards the optional feature by actual symbol availability. |
| !3569 | merged / scanned | Diameter AVP/enumeration dictionary updates; specification maintenance. |
| !3568 | merged / checker corroboration | PIM extension work receives explicit typed-item feedback: FT_IPv4/IPv6 tree additions must use compatible encoding semantics. Jaap Keuter directs the contributor to the exact checker-reported API/field mismatch. |
| !3567 | closed / superseded | release-3.4 ring-buffer fix was already cherry-picked in !3565; Pascal Quantin closes the duplicate. No implementation weight. |
| !3566 | merged / Guy Harris backport | Guy Harris-authored master-3.2 backport of ring-buffer packet-count validation; corroborates !3563. |
| !3565 | merged / Guy Harris backport | Guy Harris-authored release-3.4 backport of ring-buffer packet-count validation; corroborates !3563. |
| !3564 | closed / superseded | Earlier Kerberos ticket-signature draft explicitly closed as superseded by merged !3570; down-weighted. |
| !3563 | merged / master | Makes Wireshark/TShark ring-buffer validation accept packet-count rotation just as dumpcap does; keeps front-end option validation consistent. |
| !3562 | merged / backport | master-3.2 NR RRC MeasTriggerQuantityOffset formatting fix updates ASN.1 config/template plus regenerated output; generated-source discipline corroboration. |
| !3561 | merged / backport | release-3.4 NR RRC MeasTriggerQuantityOffset formatting fix updates authoritative generator inputs and generated output together. |

## Strongest durable findings

1. **Optional external-library behavior must be gated by symbols that are actually available to link on supported platforms.** !3570 only became portable after Windows exposed that Kerberos encode/decode functions available in one environment were not exported there; the accepted solution adds configure-time function checks and feature guards.
2. **Compiler/linker feature availability is not a substitute for workflow validation.** !3571 reverts CET/EHCONT flags that were nominally supported by the MSVC version but broke incremental compilation/linking.
3. **Checker warnings are evidence to investigate, not a substitute for protocol semantics.** !3576 deliberately treats suspicious mask width/notation as warning-like because wider masks can be intentional; !3591 and !3577 show the same checker family finding both cosmetic forms and real field/type/width defects.
4. **Name state for what it is, not for an assumed actor.** Guy Harris-authored !3588 and !3590 replace 'user/edited/changed' block terminology with 'modified' because changes can originate from Lua or previous users and not from an editing action.
5. **Dependency layering must avoid cycles and accidental public-contract breakage.** !3602 is an early wmem-independence discussion with Guy Harris explicitly testing the intended layering direction. !3603 is useful negative history: hiding GLib usage requirements broke external plugin builds and was later repaired.
6. **Keep generated dissector inputs authoritative.** !3582–!3584 and !3561–!3562 update ASN.1/config/template sources together with generated C; they corroborate the established rule that generated output is not the primary edit surface.
7. **Reviewability and branch hygiene matter.** Closed !3598 records Anders Broman warning against one huge/unrelated Thrift MR; !3575/!3587 show the practical topic-branch/amend workflow and successful successor with a sample capture.
8. **Closed malformed-input work is guidance, not accepted implementation precedent.** !3586's loop-bound/default-case review from Gerald Combs is useful historical evidence, but the MR was superseded by later !26411 and is deliberately down-weighted here.

No SMPTE 291/VANC packet type was encountered in this batch.
