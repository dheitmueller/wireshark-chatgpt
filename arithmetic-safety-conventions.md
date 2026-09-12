# Wireshark Arithmetic Safety Conventions

This file records durable integer and arithmetic-safety conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Prove the range of intermediate expressions, not only the final result

An arithmetic expression can have a mathematically valid final result while overflowing in an intermediate operation performed in the operand type. Widen the operands or restructure the expression before the potentially overflowing step; assigning the already-overflowed result to a wider destination is too late.

Merged MR !26329, authored and merged by John Thacker, fixes the helper that calculates storage required for 7-bit encoded characters. The final byte count fits its result type, but an intermediate multiplication/addition could overflow before division. The accepted fix performs the intermediate calculation in a wider type. Release backports !26332 and !26333 preserve the same correction.

Merged MR !26331 independently applies the same principle to sharkd resampling: a large change in sampling rate could overflow the intermediate product even though the eventual resampled index was representable. Release backports !26334 and !26335 carry that fix to stable branches.

**Implementation rule:** establish the maximum value of every intermediate expression in packet-, file-, and user-controlled arithmetic. Cast or widen before multiplication/addition, or algebraically divide/reduce before the growth step when that preserves exact semantics. Do not infer safety merely from the range of the final quotient or destination variable.

**Confidence:** Very high. Two merged master fixes authored and merged by John Thacker, each propagated to stable branches.

## Small unsigned integer operands can still undergo signed arithmetic

Declaring a value as `uint8_t` or `uint16_t` does not guarantee that an arithmetic expression involving it is evaluated as unsigned. C's usual integer promotions can promote an unsigned type whose rank is below `int` to signed `int` when all of its values fit, so multiplication or other arithmetic can still invoke signed-overflow undefined behavior before assignment to an unsigned destination.

Merged MR !22808, authored and merged by John Thacker after a Coverity finding, fixes exactly this pattern in the openSAFETY dissector. The MR explicitly notes that making an operand an unsigned type of smaller rank than `int` does not prevent the promotion, and that using an appropriately ranked type does not cost performance in practice.

**Implementation rule:** reason about the promoted type of each operand, not merely its declared typedef. When arithmetic must have unsigned or wider semantics, arrange for the expression itself to be evaluated in a sufficiently ranked unsigned/wide type before the potentially overflowing operation.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker with an explicit language-semantics rationale.

## Reject invalid divisors and other arithmetic invariants at the boundary where they are established

Values loaded from capture metadata should be validated before they become arithmetic operands. If a format requires a nonzero scaling factor, reject zero while opening/parsing the metadata rather than allowing later code to divide by it and produce platform-dependent exceptions or meaningless results.

Merged MR !26300, authored and merged by John Thacker and approved by Anders Broman, rejects NetXRay captures whose `realtick` produces `ticks_per_sec == 0.0`, and retains a second internal-invariant check immediately before division. The MR notes that default builds may merely produce bogus timestamps rather than trap, which makes explicit validation necessary even when a crash is not observed.

**Implementation rule:** validate divisors, shifts, dimensions, and scale factors as soon as their protocol/file representation is decoded. Keep a defensive assertion/error check at the arithmetic use site when violating the invariant would otherwise yield undefined, implementation-dependent, or silently nonsensical results.

**Confidence:** Very high. Merged master wiretap fix with explicit Anders Broman approval.

## Use canonical alignment helpers instead of open-coded roundup arithmetic

Alignment expressions are deceptively easy to get wrong at the already-aligned boundary. Prefer Wireshark's shared roundup helpers when they express the required alignment rather than maintaining a local bit-arithmetic formula.

Merged master MR !24846, authored by John Thacker and approved and merged by Jaap Keuter, fixes GDSDB's 32-bit alignment helper. The local expression `length + (4 - (length & 3))` incorrectly advanced values that were already divisible by four; replacing it with `WS_ROUNDUP_4(length)` restored the protocol's required layout. The same correction was deliberately propagated to release-4.6 and release-4.4 in merged MRs !24850 and !24851.

**Implementation rule:** when rounding lengths, offsets, or allocation sizes to a standard boundary, use the project's canonical helper where one exists. If a local formula is unavoidable, test the zero-remainder case explicitly as well as values immediately below and above the boundary.

**Confidence:** Extremely high. Merged master correctness fix from John Thacker with two merged stable-branch backports.

## Constrain semantic range before applying growth-producing transforms

A wire field can have a much wider representational range than the protocol operation that consumes it. If later code multiplies, shifts, or otherwise expands that value, validating it after the transform is too late: the dangerous intermediate arithmetic has already happened.

Merged master MR !24809, authored by John Thacker and approved and merged by Anders Broman, fixes RTPS domain-ID handling exposed by OSS-Fuzz. `DomainId_t` is carried as a 32-bit value, while the RTPS port-mapping scheme only admits a small domain range. The accepted fix moves the semantic range check ahead of `domain_id * DOMAIN_GAIN`, preventing fuzz-controlled values from overflowing during port-offset calculation.

**Implementation rule:** distinguish a field's encoded width from the narrower range accepted by the operation that uses it. Normalize or reject out-of-range values before multiplication, offset derivation, indexing, allocation sizing, or another growth-producing transform; do not rely on a clamp performed afterward.

**Confidence:** Very high. Merged master fuzz-found arithmetic fix authored by John Thacker with Anders Broman approval.

## Do not negate the minimum value of a signed integer type

Two's-complement signed integer ranges are asymmetric: the most-negative value has no corresponding positive value in the same type. Code that computes an absolute magnitude with `-value` therefore overflows for `INT_MIN`/`INT32_MIN`, even if every other negative input works.

Merged master MR !24740, authored by John Thacker and approved/merged by Anders Broman after OSS-Fuzz finding 494034581, fixes `signed_time_msecs_to_str()` without ever negating the full negative input. It relies on the C99/C++11 rule that signed division and remainder truncate toward zero: the code computes the remainder while the value is still negative, negates only that small bounded remainder, and divides the original signed value directly. Merged stable-branch backports !24744 and !24745 carry the same correction.

**Implementation rule:** when an input may span the entire signed type, never obtain its magnitude by negating it in that type. Restructure the calculation so each negated quantity is provably representable, or convert through a deliberately chosen unsigned/wider representation with well-defined semantics.

**Confidence:** Extremely high. Merged OSS-Fuzz-found master fix authored by John Thacker and propagated to two maintained branches.

## Do not use `FLT_MIN` as the most-negative floating-point sentinel

The floating-point `*_MIN` constants are not analogous to integer `INT_MIN`: for ordinary IEEE-style floating types, `FLT_MIN` is the smallest positive normalized `float`, not the most-negative representable value. Initializing a running maximum to `FLT_MIN` therefore fails for a data set whose values are all negative. Use `-FLT_MAX` (or the language/library equivalent of `lowest()`) when a most-negative finite sentinel is required.

Merged master MR !23220 fixes the stats-tree floating maximum accumulator from `FLT_MIN` to `-FLT_MAX`, specifically because statistics containing only negative values were otherwise reported incorrectly. The change was merged by John Thacker.

**Implementation rule:** when initializing floating min/max accumulators, choose an extremum whose semantics match the direction of the reduction. Do not assume integer-style meanings for `FLT_MIN`, `DBL_MIN`, or their equivalents.

**Confidence:** High. Direct merged correctness fix with an explicit explanation of the C floating-point constant semantics.

## Do not use unsigned post-decrement as the loop condition

A condition such as `while (count--)` still performs the decrement when the old value is zero. For an unsigned counter, that failed final test wraps the variable to its maximum value even though the loop body is not entered. The wrap may be invisible today if the counter is dead afterward, but it violates the counter's semantic invariant and can become observable after later refactoring.

Merged master MR !22439, authored and merged by Guy Harris after Coverity flagged the pattern in the COSEM dissector, replaces unsigned post-decrement loop tests with an explicit nonzero comparison and a decrement in the loop body. This makes the zero-exit path leave the counter at zero rather than wrapping it.

**Implementation rule:** for an unsigned countdown, prefer `while (count != 0) { ...; count--; }` or an equivalently explicit construct. Do not use `while (count--)` when zero is the natural terminal state, especially for values derived from packet data or retained after the loop.

**Confidence:** Extremely high. Merged master correctness cleanup authored and merged by Guy Harris, with the precise unsigned-wrap behavior identified by static analysis.