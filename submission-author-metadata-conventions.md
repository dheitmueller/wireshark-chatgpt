# Submission Author Metadata and Scope Conventions

Merged MRs 3280 and 3279 contained acceptable resource-leak fixes, but Gerald Combs required the contributor to repair Git author metadata and commit-message formatting before landing. His guidance included a human-readable author identity, configured email, a brief component-oriented subject where appropriate, a blank line before an optional body, and amending the commit after fixing configuration.

Rule: pre-submit validation includes commit authorship and commit-message structure, not only source correctness.

During merged MR 3264, Guy Harris twice identified edits as unrelated to the stated compiler-version cleanup.

Rule: remove drive-by fixes and unrelated cleanup from a focused MR even when those edits are harmless. Separate scope improves review and backportability.

Confidence: very high. This is direct Gerald Combs and Guy Harris review on merged MRs.
