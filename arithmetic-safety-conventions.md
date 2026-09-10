# Wireshark Arithmetic Safety Conventions

This file records durable integer and arithmetic-safety conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Prove the range of intermediate expressions, not only the final result

An arithmetic expression can have a mathematically valid final result while overflowing in an intermediate operation performed in the operand type. Widen the operands or restructure the expression before the potentially overflowing step; assigning the already-overflowed result to a wider destination is too late.

Merged MR !26329, authored and merged by John Thacker, fixes the helper that calculates storage required for 7-bit encoded characters. The final byte count fits its result type, but an intermediate multiplication/addition could overflow before division. The accepted fix performs the intermediate calculation in a wider type. Release backports !26332 and !26333 preserve the same correction.

Merged MR !26331 independently applies the same principle to sharkd resampling: a large change in sampling rate could overflow the intermediate product even though the eventual resampled index was representable. Release backports !26334 and !26335 carry that fix to stable branches.

**Implementation rule:** establish the maximum value of every intermediate expression in packet-, file-, and user-controlled arithmetic. Cast or widen before multiplication/addition, or algebraically divide/reduce before the growth step when that preserves exact semantics. Do not infer safety merely from the range of the final quotient or destination variable.

**Confidence:** Very high. Two merged master fixes authored and merged by John Thacker, each propagated to stable branches.

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