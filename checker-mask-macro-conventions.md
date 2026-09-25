# Checker Mask Macro Conventions

Merged !10315 extends `check_typed_item_calls.py` so ordinary named mask macros are substituted before field metadata checks. The stronger analysis exposed real mask-width and Boolean registration issues that were invisible when only literal masks were understood.

**Checker rule:** repository checks should understand simple project-standard constant indirection when that indirection carries the semantics being validated. A checker that handles only literals can systematically miss the most common source style.

**Confidence:** Very high. Merged checker improvement authored and merged by Martin Mathieson, with concrete findings fixed in the same change.
