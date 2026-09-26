# Reassembly Fragment-Cache Conventions

## Keep cached fragment positions synchronized with reassembly state

Merged MR !8937 shows that remembering the first unresolved gap can greatly reduce repeated traversal of long fragment lists.

**Rule:** treat cached positions and contiguous-length summaries as derived state. Whenever the underlying fragment list changes, refresh or invalidate those summaries before they are used again.

**Testing:** cover less-common list-change paths as well as ordered insertion. !8937 used issue captures and reassembly stress tests before merge.

**Confidence:** Very high; merged common-reassembly work with John Thacker review.
