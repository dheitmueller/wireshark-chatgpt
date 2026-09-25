# Review findings: Wireshark MRs !9863-!9912

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

The reviewed set was rebuilt from all available per-run ledgers plus reviewed-mrs.md on the latest completed review branch. Candidate MR numbers were checked individually. The historical !17571-!17620 ledger still contains exactly 50 unique MRs. This batch is exactly !9912 through !9863: 49 merged and one closed/unmerged (!9907).

## Promoted findings

- !9894, !9895, !9904: frame dependencies are derived dissection state, must be cleared on redissection, must preserve nested/transitive dependencies, and benefit from set/hash semantics for large reassemblies. Added to frame-dependency-conventions.md.
- !9901, !9912: copy-pasted Wireshark and Logray behavior must receive equivalent fixes. Gerald Combs and John Thacker explicitly requested cross-frontend parity. Added to shared-helper-ownership-conventions.md.
- !9882, !9887: Gilbert Ramirez recommended representing TECMP voltage numerically; merged master !9887 uses FT_DOUBLE plus voltage units so numeric display-filter comparisons work. Added to numeric-field-representation-conventions.md.
- !9889: John Thacker moved color-filter errors to configuration-load time with precise parser context and aggregated warnings; Stig Bjørlykke approved the early warning for this case. Added to configuration-diagnostic-conventions.md.
- !9906: a UDS correctness fix was deliberately split from cleanup so the fix could be backported independently. Added to submission-backport-scope-conventions.md.
- !9907: Jaap Keuter rejected an automatic release-branch update that extended the Asterix dissector; Gerald Combs corrected the automation to keep that feature-bearing generation on master. The closed MR is retained only as release-policy evidence.
- !9863 and !9864: Guy Harris replaced a terse GMR-1 workaround comment with detailed ETSI specification context explaining the ambiguity. Added to source-comment-conventions.md.

## Corroboration

!9892 reinforces the generated-source rule by moving ASN.1 output fixes back into templates. !9881 shows typed-item checker findings uncovering real width/access defects. !9865 reinforces representative captures, release notes for new dissectors, specification-derived terminology, and generated-item presentation conventions. !9875 reinforces capture-driven review and existing item-return helpers. !9877 adds temporal context to wrapped PTP identifiers. !9870 uses explicit validity for optional capture-relative time. The UDS zero-length fixes !9897, !9900, !9905, and !9910 reinforce existing parser precondition guidance.

The remaining MRs were inspected but did not justify additional durable rules beyond stronger existing notebook guidance.

Exact membership is recorded in reviewed-mrs-automation/reviewed-mrs-automation-9863-9912.md.

Frontier probe only: !9862, "gryphon: fix handling the ioctl code in a response.", exists and is merged; it is not counted in this run.
