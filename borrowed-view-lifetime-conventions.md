# Borrowed-View Lifetime Conventions

Merged MR !10979, authored and merged by John Thacker, provides strong evidence that a zero-copy UI view must not outlive the backing storage it references. The accepted fix keeps the borrowed view while its source is valid, then explicitly detaches it at the source-owner teardown boundary so the persistent UI object has independent storage.

**Implementation rule:** distinguish the lifetime of a wrapper or view from the lifetime of its backing storage. If a retained UI object can cross the backing owner's teardown boundary, detach or copy the data before that boundary completes rather than waiting for a later access.

**Confidence:** Very high.
