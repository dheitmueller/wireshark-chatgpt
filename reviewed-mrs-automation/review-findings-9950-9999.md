# Review findings for !9950-!9999

The batch contained 48 merged MRs and two closed MRs (!9977 and !9976). Merged work was weighted more heavily.

Strongest findings:
- !9996: reassembly identity must include every field needed to distinguish concurrent messages, including endpoint context where necessary.
- !9991: a heuristic dissector should report success only when it actually claims the payload.
- !9968 and !9999: validate structured-output state at the transition that can violate the invariant so later diagnostics remain accurate.
- !9963-!9965: keep success-with-warning distinct from failure.
- !9977: maintainer guidance confirms that new feature work belongs on master rather than stable release branches.
- !9957: derive extensible UI capability from the runtime registry rather than a second hard-coded protocol list.
- !9950: later packet review must use the historical state that applied to that packet, not the newest conversation state.

The empty corpus artifact for !9976 was checked against upstream metadata and was treated as closed, low-weight history.

The corpus remained at `ddcaa22b51c68f594e425a23388c3a2086813054` after review.
