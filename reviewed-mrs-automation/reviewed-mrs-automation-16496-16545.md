# Wireshark MR automation review ledger — !16496 through !16545

Corpus commit: `08420a2f7851841b7a38714f642987c8cd167184`

This run rebuilt the already-reviewed set from `reviewed-mrs.md` plus the complete set of per-run files in `reviewed-mrs-automation/`, preserving and counting the historical !17571-!17620 batch. Corpus enumeration used the complete repository tree rather than a paginated directory listing. Selection was by individual MR number: corpus MRs minus the exact reviewed set, sorted numerically descending.

Exactly 50 previously unreviewed corpus MRs were reviewed, in descending priority/order:

- !16545, !16544, !16543, !16542, !16541, !16540, !16539, !16538, !16537, !16536
- !16535, !16534, !16533, !16532, !16531, !16530, !16529, !16528, !16527, !16526
- !16525, !16524, !16523, !16522, !16521, !16520, !16519, !16518, !16517, !16516
- !16515, !16514, !16513, !16512, !16511, !16510, !16509, !16508, !16507, !16506
- !16505, !16504, !16503, !16502, !16501, !16500, !16499, !16498, !16497, !16496

Outcome weighting followed notebook policy: merged work was treated as accepted evidence; closed/abandoned work was down-weighted. In particular, !16539 and !16506 were closed unmerged, and !16509 was an empty/conflicted closed draft, so none of those is treated as an implementation exemplar.

Durable findings added to the notebook:

- !16531 (Guy Harris, merged) replaces multiple positional booleans that jointly encode a PIDL policy-handle operation with named semantic flags. Added `api-mode-parameter-conventions.md`.
- !16517 (John Thacker, merged) demonstrates that `tcp_dissect_pdus()` framing callbacks must bound variable-length prefixes, distinguish legal truncation from malformed overlength, and avoid exception/undefined-shift behavior. Added to `tcp-desegmentation-conventions.md`.
- !16507, with stable backports !16518 and !16519, shows that protocol-tree display parameters are semantic enum values, not field widths that merely fit the same C parameter type. Added to `field-registration-conventions.md`.
- !16504 (Guy Harris, merged) makes `tvbuff.h` include the headers that directly define annotations/export macros it uses instead of relying on transitive include order. Added to `public-header-validation-conventions.md`.

Other accepted MRs primarily corroborated existing notebook guidance: !16532 is a direct accessor-width correctness fix (`letohs` versus `letohl`); !16521 reinforces editing generator/configuration sources and regenerating generated outputs; !16524 reinforces reuse of existing conversion/value-string helpers; !16540 and !16545 provide cross-platform documentation-build portability evidence; !16543 shows that a capture is strongly preferred for dissector changes but a narrowly mirrored/spec-aligned implementation can still merge when a capture is unavailable.
