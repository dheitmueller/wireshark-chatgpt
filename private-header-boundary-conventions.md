# Private Header Boundary Conventions

Merged master MR 5034 states that epan/ftypes/ftypes-int.h is private to epan/ftypes. The accepted change moves display-filter code, WSLua, protocol-tree and printing code, rawshark, and generators to the public ftypes header and public functions or accessors.

Rules:
- Do not solve a missing cross-subsystem operation by including another subsystem's *-int.h file.
- Expose the smallest public function or accessor that represents the supported operation.
- Prefer public functions over macros that require callers to know private structure layout.
- Treat a private-header include outside its owning subsystem as an architectural smell during review.

Confidence: very high. Merged master refactor with the boundary stated explicitly in the MR description.
