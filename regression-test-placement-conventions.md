# Wireshark Regression-Test Placement Conventions

This file records durable conventions for placing regression coverage when fixing defects. Current upstream test organization remains authoritative.

## Put a regression test in the suite that owns the broken invariant

A focused bug fix in a reusable utility or core data structure should normally add the smallest regression to the existing test suite responsible for that API's observable invariants. The goal is not merely to exercise the modified line; it is to lock down the public property that was wrong and to make the test fail on the buggy implementation.

Merged master MR !12515 fixed `wmem_list_insert_sorted()` failing to increment the list's recorded count. Jaap Keuter explicitly requested an addition to `wsutil/wmem/wmem_test.c:wmem_test_list()`, saying that this was where the defect should have been caught. The fix merged with that review direction and was then backported in !12542 and !12543.

**Testing rule:** when correcting a core helper/container invariant, identify the existing unit-test family that owns that API and add a regression there. Assert the externally meaningful state or result, not merely the internal implementation detail changed by the patch.

**Review rule:** if a defect in a mature helper could have been detected by an existing unit-test family, treat missing regression coverage as part of the bug rather than optional follow-up cleanup. This is especially important for invariants such as counts, ordering, ownership, boundary behavior, and lifecycle transitions.

**Confidence:** High. Direct maintainer review on a merged master fix, followed by stable-branch backports of the corrected behavior.

## Assert empty-input ownership invariants for composite TVBuffs

Merged !9359 demonstrates a real composite-TVBuff edge case in HTTP/2 and CBOR: a new composite is only properly managed after at least one member has been appended or prepended, while finalizing an empty composite can throw. The accepted implementation lazily constructs composites on the first nonempty segment; for an otherwise empty indefinite-length CBOR string, it supplies a zero-length subset TVB so callers still receive the expected non-NULL byte-string view. The same MR extends wscbor_test.c with an empty indefinite CBOR regression and supplies a deliberately malformed HTTP/2 capture plus Valgrind reproduction.

**Testing rule:** cover zero-member, one-member and malformed-input paths of container construction APIs, including their allocation/finalization behavior and public non-NULL/empty-value contracts, not only normal populated containers. **Confidence:** very high (merged fix with focused unit test and memory diagnostic reproduction).

## Test third-party decoder outputs and edge-case result codes, not just a successful return

Merged master **!8973** put SAP LZC/LZH support in `wsutil` and extended `wsutil/test_wsutil.c` with compressed fixtures, expected decoded bytes and error cases. **Michael Mann** identified a copied Python fixture's missing trailing space and opposed simply deleting a failing LZC test: a mismatch might mean the algorithm is genuinely wrong. The finished test exercises both algorithms and several invalid input/output conditions.

**Rule:** keep known compressed input plus exact expected output in a utility-level unit test for a shared decoder; test mismatch, zero/too-small output capacity, invalid magic, unknown algorithm and other returned error codes. A green dissection smoke test alone does not validate decoder correctness.

**Confidence:** Very high, accepted code with direct Michael Mann regression-test scrutiny.
