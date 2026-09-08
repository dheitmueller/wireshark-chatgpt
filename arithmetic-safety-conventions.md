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