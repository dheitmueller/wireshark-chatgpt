# Wireshark MR Review Automation Ledger — !24409–!24458

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all available per-run files under `reviewed-mrs-automation/`, using individual MR entries rather than assuming whole numeric ranges were reviewed. The historical !17571–!17620 batch remains explicitly preserved and counted. Combined tracking contained no explicit review entries for !24409–!24458, so these are the fifty highest-numbered corpus MRs absent from the reconstructed reviewed set.

Exact reviewed MR set, in review order (newest to oldest):

!24458, !24457, !24456, !24455, !24454, !24453, !24452, !24451, !24450, !24449,
!24448, !24447, !24446, !24445, !24444, !24443, !24442, !24441, !24440, !24439,
!24438, !24437, !24436, !24435, !24434, !24433, !24432, !24431, !24430, !24429,
!24428, !24427, !24426, !24425, !24424, !24423, !24422, !24421, !24420, !24419,
!24418, !24417, !24416, !24415, !24414, !24413, !24412, !24411, !24410, !24409.

Count: **50**.

## Promoted durable findings

- **!24450 — C expression sequencing:** John Thacker explicitly rejected assigning the same scalar through multiple function-call arguments because C argument evaluation is unspecified/unsequenced. Promoted to `c-expression-sequencing-conventions.md`.
- **!24449 + !24419 — mixed exception systems:** WSLua must not let Lua `longjmp` escape through an active libwireshark/epan TRY frame; validate before crossing the boundary, with exception-stack reset only as last-resort crash containment. Promoted to `exception-boundary-conventions.md`.
- **!24424 — persisted preference compatibility:** Michael Mann required an upgrade path when replacing the old NTLMSSP password preference with a credentials UAT; obsoleting a key alone can make its saved value inaccessible. Promoted to `preference-migration-conventions.md`.
- **!24425 — generated-resource ordering:** review exposed a latent QRC/translation race where generated `.qm` inputs were not explicitly ordered before resource generation. Promoted to `generated-resource-dependency-conventions.md`.

## Strong corroboration retained without duplicate notebook rules

- !24458, !24457, !24454, !24443: checked length/arithmetic and VLAN/tag bounds handling already covered by arithmetic/bounds guidance.
- !24453, !24452, !24421: fixed-buffer truncation and project-native string-buffer handling reinforce existing bounded text/string guidance.
- !24448, !24447, !24445, !24420, !24418: cleanup and scoped-allocation fixes reinforce existing memory-lifetime/TVB-exception guidance.
- !24446: Guy Harris's review questions the assumption that a non-Windows/non-macOS fallback is necessarily "Linux" and highlights desktop-environment-specific theme behavior; this strongly corroborates existing portability guidance to model the actual capability/platform domain rather than an incidental OS label.
- !24437, !24455, !24442: Buffer-based prefix construction reinforces existing container/buffer guidance.
- !24412: resets parameter-width state before each RTPS parameter and fixes per-fragment offsets; useful state/reassembly corroboration.
- !24417 was a closed draft predecessor and was deliberately down-weighted relative to the subsequently merged WSLua debugger work.

Merged MRs were weighted above closed/draft/superseded work; substantive maintainer comments were weighted by reviewer authority, with Guy Harris and other core maintainers receiving correspondingly greater evidentiary weight.
