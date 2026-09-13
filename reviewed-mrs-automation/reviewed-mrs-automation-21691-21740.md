# Wireshark MR automation review: !21691-!21740

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to oldest
MRs reviewed in this run: 50

Selection was built from the union of all existing `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md`, rather than assuming numeric ranges were complete. The historical !17571-!17620 batch remains part of the already-reviewed set. No previously reviewed sparse exception occurred inside this batch, so the fifty highest-numbered unreviewed corpus entries were exactly !21740 through !21691.

## Exact reviewed MR set

- !21740
- !21739
- !21738
- !21737
- !21736
- !21735
- !21734
- !21733
- !21732
- !21731
- !21730
- !21729
- !21728
- !21727
- !21726
- !21725
- !21724
- !21723
- !21722
- !21721
- !21720
- !21719
- !21718
- !21717
- !21716
- !21715
- !21714
- !21713
- !21712
- !21711
- !21710
- !21709
- !21708
- !21707
- !21706
- !21705
- !21704
- !21703
- !21702
- !21701
- !21700
- !21699
- !21698
- !21697
- !21696
- !21695
- !21694
- !21693
- !21692
- !21691

## Durable finding promoted

- **!21738 — Deep / merged master / high weight.** WSLua `TreeItem_set_len` gained range validation. Jaap Keuter explicitly questioned whether arbitrary negative lengths should be accepted merely because `-1` has special meaning. The accepted revision permits the defined `-1` sentinel and rejects values below it. Added this refinement to `api-domain-conventions.md`: a distinguished sentinel does not confer meaning on the whole otherwise-invalid numeric region.

## Strong corroborating and negative evidence

- **!21734 — Deep / merged master / high weight.** John Thacker fixes OSS-Fuzz signed-overflow/bounds behavior in WCCP by making a `proto_tree_add_item_ret_uint()` call execute even when the protocol tree is absent. This reinforces the existing principle that parser validation and bounds checking must not disappear merely because tree construction is disabled.
- **!21726 — Deep / merged master, but negative lifecycle evidence.** A GRegex leak cleanup freed match information that was still semantically needed for IMSI association; a later post-merge report says the state had to be restored in !22209. This reinforces existing ownership/lifetime guidance: before freeing data during leak cleanup, trace whether any returned pointer or derived state escapes into packet/session state.
- **!21706 — Deep / merged backport.** Public dissector headers switch from local `"packet.h"` includes to installed-path `<epan/packet.h>` includes so the headers can actually be shipped and consumed externally. Strong corroboration of existing public-header self-containment/install-path guidance.
- **!21704 — Deep / merged master.** Alexis La Goutte explicitly requests pcaps for new Kafka protocol support; the contributor supplies captures for protocol versions 0 and 1 before approval. Strong corroboration of the existing representative-capture expectation for protocol features.
- **!21701 — Deep / merged master.** Anders Broman catches an `FT_BOOLEAN` registration whose mask/declared width does not match the expanded CQLv5 wire representation; the contributor reworks the flags. Corroborates semantic field-type/mask-width guidance.
- **!21693 — Discussion-focused / merged master.** Alexis La Goutte flags Clang analyzer dead stores, and subsequent IDE/static-analysis warnings are discussed before the ASTERIX update is accepted. Corroborates the established static-analysis/pre-submit workflow.

## Per-MR audit notes

- !21740 — merged release-4.6 zstd packaging revert; release/build maintenance, no new rule.
- !21739 — merged master zstd packaging revert; build maintenance, no new rule.
- !21738 — merged WSLua length-domain validation; promoted exact-sentinel guidance.
- !21737 — merged ORAN FH CUS fixes for previously untested SE28 behavior; testing-gap corroboration only.
- !21736 — merged const-ification of function-pointer arrays; immutable-data cleanup.
- !21735 — merged SVCCTL automatic policy-handle tracking through PIDL configuration; generated-source/state corroboration.
- !21734 — merged John Thacker WCCP fuzz fix; safety validation independent of protocol-tree construction.
- !21733 — merged SVCCTL authoritative IDL annotation update; generated-source corroboration.
- !21732 — merged ORAN field-label cleanup; presentation maintenance.
- !21731 — merged release-4.6 SVCCTL IDL backport; no independent lesson.
- !21730 — merged release-4.6 GSM SIM backport; no independent lesson.
- !21729 — merged `find_filter_matches.py` test adjustment for usable tshark; test tooling maintenance.
- !21728 — merged TLS/DTLS static-analyzer dead-store cleanup; static-analysis corroboration.
- !21727 — merged GSM SIM master improvement; protocol-specific semantics.
- !21726 — merged GRegex leak cleanup later reported to lose IMSI state; negative ownership/lifetime evidence, not treated as an exemplar.
- !21725 — merged SVCCTL generated IDL additions, validated with a sample capture; generated-code/testing corroboration.
- !21724 — merged release-4.2 macOS arm64 pytest CI adjustment; CI maintenance.
- !21723 — merged JSON-log timestamp presence flag fix; record flag/value consistency, no new broad rule.
- !21722 — merged 4.2.14 release notes preparation; release maintenance.
- !21721 — merged 4.4.10 release notes preparation; release maintenance.
- !21720 — merged release-4.4 macOS arm64 pytest CI adjustment; CI maintenance.
- !21719 — merged ORAN beam-forming fixes; protocol-specific.
- !21718 — merged release-4.2 CFM Sender ID length backport; no independent lesson.
- !21717 — merged release-4.4 CFM Sender ID length backport; no independent lesson.
- !21716 — merged release-4.6 CFM Sender ID length backport; no independent lesson.
- !21715 — merged release-4.6 GSM SIM SW2 length semantics backport; no independent lesson.
- !21714 — merged Gerald Combs GitLab RPM-build verbosity reduction to stay under runner log limits; CI resource maintenance.
- !21713 — merged release-4.6 Stratoshark missing initializer fix after callback-table expansion; API-structure evolution corroboration.
- !21712 — merged spelling cleanup; maintenance-only.
- !21711 — merged master CFM Sender ID TLV length fix by Jaap Keuter; protocol-specific length semantics.
- !21710 — merged backport removing obsolete macOS CI CMake hack; maintenance-only.
- !21709 — closed empty draft release MR; no implementation evidence.
- !21708 — merged release-4.6 SGP32 direct string-column helper cleanup; local style/API simplification.
- !21707 — closed malformed/superseded cherry-pick of the SGP32 cleanup; down-weighted.
- !21706 — merged public-header installed include-path fix; strong public-header corroboration.
- !21705 — merged release-4.6 Qt application-icon loading backport; UI/platform maintenance.
- !21704 — merged Kafka client-quota protocol support; maintainer-requested pcaps supplied before approval.
- !21703 — merged master GSM SIM SW2 zero-means-256 handling; protocol-specific semantics.
- !21702 — merged release-4.2 CFM ingress-alias off-by-one backport; no independent lesson.
- !21701 — merged CQLv5 improvements; maintainer catches boolean field mask/width mismatch; field-semantics corroboration.
- !21700 — merged H.248 const-ification; immutable-data cleanup.
- !21699 — merged PLDM firmware-parameter command support; protocol feature, little independent review guidance.
- !21698 — merged master Qt icon-loading fix; platform/UI maintenance.
- !21697 — merged release-4.2 reverse icon-loading-order backport; no independent lesson.
- !21696 — merged release-4.4 reverse icon-loading-order backport; no independent lesson.
- !21695 — merged release-4.6 reverse icon-loading-order backport; no independent lesson.
- !21694 — merged master reverse icon-loading-order fix; Qt/platform-specific behavior.
- !21693 — merged ASTERIX update with static-analysis warnings raised during review; static-analysis corroboration.
- !21692 — closed duplicate of !21681 after the accepted successor fix was judged good; superseded and down-weighted.
- !21691 — merged release-4.2 macOS design-compatibility backport; platform/release maintenance.

## Weighting notes

Merged master changes were treated as the strongest implementation evidence. Stable-branch backports and repeated cherry-picks were reviewed but not allowed to multiply the evidentiary weight of the same underlying change. Closed !21709, !21707, and !21692 were explicitly down-weighted; !21692 points to accepted successor !21681 and !21707 was superseded by merged !21708. The post-merge regression reported on !21726 was retained as negative ownership/lifetime evidence rather than promoting the merged cleanup as a positive pattern. No MR in this run contained Guy Harris feedback that materially changed the extracted rules, so no artificial Guy-weight was assigned.
