# Formatter Allocation API Conventions

Merged master MR 5017 changes ftype representation callbacks from caller-provided buffers to strings allocated from a caller-selected wmem scope, and removes fvalue_string_repr_len().

Guy Harris explicitly explains that once formatting changes from filling a supplied buffer to dynamically allocating the result with wmem, the separate length-query operation is no longer needed. He also asks that this architectural reason be stated in the commit and merge-request descriptions; Joao Valverde updates the series accordingly.

Rule: when the callee owns exact sizing and allocation, remove a redundant preflight-size interface instead of maintaining two size formulas that can drift.

Rule: make the allocation scope part of the contract so result lifetime is explicit.

Submission rule: explain the ownership/design reason behind a mechanical-looking API change.

Confidence: extremely high. Merged master change with direct Guy Harris review.
