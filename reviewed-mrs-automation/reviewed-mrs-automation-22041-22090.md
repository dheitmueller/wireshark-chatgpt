# Automated Wireshark MR review: !22041 through !22090

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: newest toward older.

Selection method: rebuilt the already-reviewed set from all available files in `reviewed-mrs-automation/` plus `reviewed-mrs.md`; preserved and counted the historical !17571-!17620 batch; selected the 50 highest-numbered corpus MRs not present in that set. No isolated !220xx exclusions were found in `reviewed-mrs.md`.

Exact MRs reviewed in this run (50):

- !22090
- !22089
- !22088
- !22087
- !22086
- !22085
- !22084
- !22083
- !22082
- !22081
- !22080
- !22079
- !22078
- !22077
- !22076
- !22075
- !22074
- !22073
- !22072
- !22071
- !22070
- !22069
- !22068
- !22067
- !22066
- !22065
- !22064
- !22063
- !22062
- !22061
- !22060
- !22059
- !22058
- !22057
- !22056
- !22055
- !22054
- !22053
- !22052
- !22051
- !22050
- !22049
- !22048
- !22047
- !22046
- !22045
- !22044
- !22043
- !22042
- !22041

## Promoted durable findings

- !22081, !22082, !22083, !22087, and !22078: merged application/capture refactors reinforce explicit ownership and context flow. Product-specific policy belongs in the owning application/subclass, while reusable layers should receive application identity, options, or operation context explicitly rather than probing application flavor or mutable globals. Added to `application-layer-boundary-conventions.md`.
- !22059: merged John Thacker UAT fix establishes that recoverable per-record validation failures should preserve user configuration and keep invalid records visible/editable rather than clearing the preference table. Added to `preference-state-conventions.md`.
- !22090, !22072, and !22054: Jaap Keuter and Michael Mann review guidance plus the merged Developer's Guide update establish a focused topic-branch workflow and MR scoping by coherence: avoid submitting from a long-lived master/main branch, keep history reviewable, and do not unnecessarily fragment several small related changes into separate MRs. Added to `submission-conventions.md`. !22090 itself was closed/unmerged and is therefore treated as workflow-policy evidence rather than accepted implementation evidence.

## Strong corroborating findings already covered elsewhere

- !22042 and !22064: John Thacker-authored OSS-Fuzz fixes reinforce semantic unsigned types for nonnegative packet counts/tags and checked arithmetic (`ckd_mul`, `ckd_add`) before deriving parser lengths or offsets.
- !22058: resource/complexity limits should report their actual condition and preserve useful partial dissection where safe rather than masquerading as a packet bounds error.
- !22075: stateful dissector validation should include partial/midstream captures, not only complete sessions with all setup exchanges present.
- !22074 and !22068: `check_dissector.py` findings are useful pre-submission validation, including field-mask correctness and other structural issues.
- !22084: avoid registering a specialized dissector on overly generic media types that would cause it to claim unrelated content.
- !22047 and !22048: a success return that uses output parameters must leave those outputs initialized and semantically valid.
- !22043: generator/tooling code should detect unsupported or missing source constructs explicitly and emit one actionable source-location diagnostic rather than cascading implementation-language warnings.

## Weighting notes

Merged master MRs and explicit maintainer-authored/maintainer-reviewed changes were weighted most heavily. Stable-branch backports were treated mainly as corroboration of an already accepted fix. Closed/unmerged !22090 was deliberately down-weighted for implementation conclusions while retaining Jaap Keuter's explicit submission-process guidance.
