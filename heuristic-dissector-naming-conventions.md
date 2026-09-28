# Heuristic Dissector Naming Conventions
Merged MR 5966 renamed heuristic dissector UI labels. Roland Knall cautioned that heuristic registration is about uncertain dispatch, not necessarily a canonical X-over-Y protocol name, and that user-visible names can be relied on in documentation. The most contentious renames were reverted before merge.

Rule: treat human-readable heuristic names as user-facing semantic API. Do not infer a canonical label solely from the dissector table or carrier protocol; preserve established or documented terminology unless protocol semantics justify the change.

Confidence: high; merged change after explicit maintainer review and partial reversion.
