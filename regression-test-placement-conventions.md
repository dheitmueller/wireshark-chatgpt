# Wireshark Regression-Test Placement Conventions

This file records durable conventions for placing regression coverage when fixing defects. Current upstream test organization remains authoritative.

## Put a regression test in the suite that owns the broken invariant

A focused bug fix in a reusable utility or core data structure should normally add the smallest regression to the existing test suite responsible for that API's observable invariants. The goal is not merely to exercise the modified line; it is to lock down the public property that was wrong and to make the test fail on the buggy implementation.

Merged master MR !12515 fixed `wmem_list_insert_sorted()` failing to increment the list's recorded count. Jaap Keuter explicitly requested an addition to `wsutil/wmem/wmem_test.c:wmem_test_list()`, saying that this was where the defect should have been caught. The fix merged with that review direction and was then backported in !12542 and !12543.

**Testing rule:** when correcting a core helper/container invariant, identify the existing unit-test family that owns that API and add a regression there. Assert the externally meaningful state or result, not merely the internal implementation detail changed by the patch.

**Review rule:** if a defect in a mature helper could have been detected by an existing unit-test family, treat missing regression coverage as part of the bug rather than optional follow-up cleanup. This is especially important for invariants such as counts, ordering, ownership, boundary behavior, and lifecycle transitions.

**Confidence:** High. Direct maintainer review on a merged master fix, followed by stable-branch backports of the corrected behavior.
