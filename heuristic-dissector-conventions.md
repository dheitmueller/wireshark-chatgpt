# Wireshark Heuristic Dissector Conventions

This file records durable conventions for heuristic-dissector registration and default enablement extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Broad heuristics should default off when protocol metadata normally provides a stronger discriminator

A heuristic can be useful for captures in which the normal protocol discriminator is missing, but that does not imply it should run by default on every payload of the carrier protocol. When the payload syntax is generic enough to create a meaningful false-positive or performance surface, register the heuristic so users can opt in while leaving it disabled by default.

Merged MR !24231, authored and merged by John Thacker, adds JSON heuristic dissection to HTTP specifically for custom protocols that carry JSON without declaring an appropriate media type. The registration uses `HEURISTIC_DISABLE`, unlike narrower contexts where JSON heuristics are enabled. This preserves an escape hatch for underspecified traffic without making generic JSON probing part of normal HTTP dissection.

**Implementation rule:** prefer explicit media types, ports, dissector tables, or other protocol-owned discriminators when available. Add a broad heuristic as an opt-in fallback when it is valuable for nonconforming/underspecified traffic but not sufficiently selective for default use.

**Confidence:** Very high. Merged master change authored and merged by John Thacker, with the disabled-by-default behavior explicit in both the MR description and implementation.

## Heuristic probes must reject nonmatching or truncated input without throwing

A heuristic dissector is intentionally invoked on traffic that may not belong to it. Short, malformed-for-this-protocol, or otherwise nonmatching input is therefore an ordinary negative probe result, not an exceptional condition. Bounds checks in the recognition path must be written so they return false before any TVBuff access can throw.

Merged MR !25809, authored by John Thacker and merged by Anders Broman (with corresponding master/stable variants in !25807 and !25808), fixes LBM SRS recognition after an earlier safety change had been partially undone. John states the contract explicitly: heuristic dissectors must not throw exceptions on packets that do not belong to them. The fix also replaces subtraction-based length checks on unsigned offsets with `tvb_captured_length_remaining()`, because unsigned underflow can turn an invalid offset relationship into a very large value and defeat the intended test.

**Implementation rule:** keep the entire heuristic recognition path exception-safe for arbitrary carrier traffic. Before fixed-width reads, ask the TVBuff API for the captured bytes remaining at the current offset (or use another overflow-aware helper) and return `false` when insufficient. Do not prove safety with unsigned expressions such as `total - offset` or `offset - start` unless the ordering invariant has already been established independently.

**Review rule:** when a heuristic crash/bounds fix regresses later, treat the no-throw recognition contract as an invariant worth preserving explicitly rather than as a one-off local workaround.

**Confidence:** Very high. Merged fix authored by John Thacker, with the heuristic contract and unsigned-arithmetic rationale stated directly in the commit/MR and accepted on stable branches.
