# Wireshark Statistics Numeric Representation Conventions

This file records durable conventions for choosing numeric representations in statistics, graphing, and aggregation code. Current upstream source remains authoritative.

## Choose the representation according to the operation's semantic requirements

A single source field can need different internal representations for different statistical operations. Avoid forcing every operation through one type when the requirements for range and exactness differ.

Merged master MR !14970, authored and merged by John Thacker, simplifies I/O Graph item storage while preserving those distinctions. Integer totals remain `double` because repeatedly summing 64-bit integer values in a fixed-width integer accumulator can overflow, and the total is ultimately consumed by graphing as a floating-point value anyway. Losing integer precision above 2^53 is acceptable for that particular aggregate use. In contrast, minima and maxima remain 64-bit integers: converting them to `double` could change ordering for large values and therefore select the wrong frame as the true minimum or maximum.

**Implementation rule:** select accumulator and retained-value types per operation. For sums or averages ultimately presented as floating-point data, a floating accumulator can be preferable to fixed-width integer overflow when exact integer precision is not part of the observable contract. For extrema, comparisons, identifiers, and other operations where exact ordering matters, retain an exact integer representation.

**Review rule:** for each numeric conversion in statistics code, ask separately whether the operation needs exact ordering, exact arithmetic, sufficient range, or only a graphable approximation. Do not infer that a type suitable for a displayed aggregate is automatically suitable for min/max tracking or frame selection.

**Confidence:** Very high. Merged master optimization authored and merged by John Thacker with the overflow-versus-precision tradeoff stated explicitly in the MR rationale.
