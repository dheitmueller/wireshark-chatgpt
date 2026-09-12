# Automated Wireshark MR review: !22091–!22140

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: descending MR number, selecting the highest-numbered corpus MRs not already present in the notebook review tracking. Existing per-run ledgers and `reviewed-mrs.md` were consulted before selection. The historical reviewed batch !17571–!17620 remains part of the already-reviewed set.

Exact reviewed set (50 MRs):

!22140, !22139, !22138, !22137, !22136, !22135, !22134, !22133, !22132, !22131, !22130, !22129, !22128, !22127, !22126, !22125, !22124, !22123, !22122, !22121, !22120, !22119, !22118, !22117, !22116, !22115, !22114, !22113, !22112, !22111, !22110, !22109, !22108, !22107, !22106, !22105, !22104, !22103, !22102, !22101, !22100, !22099, !22098, !22097, !22096, !22095, !22094, !22093, !22092, !22091.

## Durable findings promoted

### !22129 — public extension APIs belong in dedicated public headers

Deep review, merged master, authored and merged by Guy Harris. Wiretap module-facing declarations were moved out of internal `wtap-int.h` into the dedicated public `wtap_module.h`, allowing built-in and third-party file-format modules to depend on an intentional extension API rather than private implementation details. Merged !22124 corroborates the real third-party-plugin need for a complete public header dependency chain. Promoted to `public-header-validation-conventions.md`.

### !22117 — generated source must remain reproducible from legally usable inputs

Deep review of a closed draft; down-weighted relative to merged work, but the maintainer rejection is explicit and durable. Anders Broman rejected a generated SAE J2735 dissector workflow in which Wireshark could not retain/use the ASN.1 source and would have to rely on the contributor to regenerate output externally when asn2wrs or APIs changed. Promoted as negative submission-policy evidence to `source-provenance-conventions.md`.

## Strong corroborating findings already covered by the notebook

- !22135: merged replacement of a packed native nonce structure with the exact byte-array representation required by the crypto API; corroborates `c-wire-layout-portability-conventions.md`.
- !22132: merged VLAN depth restoration on dissector exit; corroborates stack-like protocol/depth state in `dissector-state-conventions.md`.
- !22125, !22122, !22100, !22095, and !22092: merged application-flavor / UI decoupling changes; corroborate `application-layer-boundary-conventions.md`, including explicit application policy and generic abstraction boundaries.
- !22111 and !22110: merged timestamp/overflow changes favor semantic helpers and checked arithmetic over hand-rolled arithmetic; corroborate existing integer/API-safety guidance.
- !22106: merged 3GPP Nettrace recognizer uses the XML parser rather than brittle manual magic parsing; corroborates `wiretap-file-detection-conventions.md`.
- !22102 and !22101: merged LZ4 fix ensures preference/configuration state is initialized before an API reads it; corroborates initialization/lifecycle guidance.
- !22093: merged `time_t` limit correction avoids assuming `LONG_MAX` models the platform typedef; corroborates `c-type-conventions.md`.
- !22091: merged preference initialization cleanup makes initialization order explicit and removes lazy/public state checks; corroborates `initialization-lifecycle-conventions.md`.

## Weighting notes

Merged MRs were treated as the strongest implementation evidence. !22129 received especially high weight because Guy Harris both authored and merged the architectural public-API change. Open !22119 and draft/open !22105 were reviewed but not treated as accepted design. Closed drafts !22137 and !22117 were down-weighted; !22117 nevertheless contains explicit maintainer policy important enough to preserve as negative evidence. Routine generated-code refreshes, static-analyzer cleanups, backports, spelling/tooling cleanups, and narrow protocol fixes were reviewed but did not justify duplicate notebook rules.
