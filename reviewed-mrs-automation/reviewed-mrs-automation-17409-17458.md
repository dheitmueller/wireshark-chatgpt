# Automated MR review: !17409-!17458

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest unreviewed MRs. Before selection, the persistent `reviewed-mrs.md` and all available files in `reviewed-mrs-automation/` were consulted. The previously reviewed !17571-!17620 batch remains part of the reviewed set. This run contains exactly 50 MRs and no MR above !17458 was selected because those candidates were already represented in review tracking.

## Exact reviewed set

!17458, !17457, !17456, !17455, !17454, !17453, !17452, !17451, !17450, !17449, !17448, !17447, !17446, !17445, !17444, !17443, !17442, !17441, !17440, !17439, !17438, !17437, !17436, !17435, !17434, !17433, !17432, !17431, !17430, !17429, !17428, !17427, !17426, !17425, !17424, !17423, !17422, !17421, !17420, !17419, !17418, !17417, !17416, !17415, !17414, !17413, !17412, !17411, !17410, !17409.

## Durable findings

### !17410 — narrow warning suppression for generated code

Merged release-4.4 backport of accepted master behavior by John Thacker. Lemon-generated sources predictably contain unused parameters. On MSVC the build previously used `/w`, disabling every warning for those files; the accepted correction uses `/wd4100`, matching the GCC/Clang practice of suppressing only `-Wunused-parameter`. This also avoids MSVC D9025 from overriding the normal warning-level flags. Durable rule added to `build-conventions.md`: generated-code exceptions should suppress only the diagnostic the generator cannot reasonably satisfy, and only for the generated source.

### !17430 — compiler-specific behavior around assertion/check macros

Merged warning cleanup by Anders Broman. Guy Harris subsequently investigated why an argument referenced only inside a disabled `ws_return_val_if()` condition was considered unused by MSVC but not by Clang/GCC. His experiment showed that Clang's treatment depends on the exact constant-expression spelling, suggesting specialized compiler behavior rather than portable language semantics. Useful caution: warning-clean builds across one compiler family do not prove that assertion/check macros expose parameter use equivalently to another compiler. This remains contextual evidence rather than a separate notebook rule.

### !17440 — make dependency/tool discovery follow the actual environment contract

Merged master CMake change by John Thacker. MSYS2 deliberately supplies the desired Python through PATH, while CMake's Windows default can prefer registry-discovered Python. Setting `Python3_FIND_REGISTRY NEVER` for MSYS2 allows the project to use modern `FindPython3` rather than retaining deprecated `FindPythonInterp`. This reinforces the existing build convention to condition discovery on the actual toolchain/package environment rather than generic target-OS assumptions.

### !17450 — signed/unsigned boundary remains a recurring parser risk

Merged Guy Harris-authored comment flags a Bitcoin dissector question: proving a protocol length fits in `unsigned int` does not necessarily prove it can safely be cast to signed `int` before advancing a signed offset. This is high-authority corroboration of existing arithmetic-safety guidance about narrowing and signed/unsigned representation boundaries; no duplicate rule was added.

### !17458 — low-information UI rearrangement

Merged Qt packet-list column-menu rearrangement by Stig Bjørlykke. The change moves edit/resize actions ahead of alignment/display controls and contains no substantive human review discussion. Useful as accepted UI behavior but not as a durable coding convention.

## Weighting

Merged master changes were weighted most heavily. Release-branch cherry-picks were treated primarily as corroboration of their master changes. Closed, abandoned, superseded, mechanical, translation/data-only, and discussion-free changes were scanned but were not promoted into durable notebook guidance unless they supplied independent architectural/review evidence. Maintainer observations from Guy Harris, John Thacker, Gerald Combs, Pascal Quantin, Stig Bjørlykke, Anders Broman, and Martin Mathieson were weighted according to specificity and relevance rather than merely by presence.

## Notebook changes

`build-conventions.md` gained the rule **“Suppress only the diagnostic a generated source cannot reasonably satisfy”**, based on !17410. Other findings in this batch corroborated conventions already present and therefore did not justify duplicative notebook sections.

## Continuation

Rebuild the reviewed-MR set from all tracking before the next run. If no higher-numbered holes are discovered, the next descending candidate after this run is !17408.
