# Conventions from MRs !309-!358

- Validate parser helper return values before replacing the caller cursor; failure must not become later offset arithmetic (!349, !333).
- Prefer zeroed allocation when persistent state is intended to start with zero-valued scalar fields (!355, superseding !313).
- Extend source checkers to plugins where applicable, but treat heuristic findings as review input rather than automatic repair instructions (!332, !334, !346).
- Keep raw wire values distinct from display normalization and derived presentation (!317, !314, !316).
- Prefer registered `ENC_TIME_*` decoding when the wire layout directly matches it (!315).
- Keep commit validation independently rerunnable and support maintainer rebasing/minor fixes in the submission workflow (!309, !350).
