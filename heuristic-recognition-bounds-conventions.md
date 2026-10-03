# Heuristic Recognition-Bounds Conventions

## Bound variable-identifier probes by the packet and the lookup contract

A protocol can permit an identifier to vary up to a large maximum while a particular packet contains a shorter value. Heuristic recognition must not read the maximum unconditionally.

Merged master MR !571 fixes the QUIC short-header heuristic so the candidate destination connection ID is capped to captured bytes remaining after mandatory short-header framing rather than blindly copying `QUIC_MAX_CID_LENGTH`. In review, Peter Wu explains that the packet necessarily contains the flag byte, at least one packet-number byte, and a 16-byte authentication tag. He also distinguishes protocol validity from recognizability: an empty CID is legal, but the existing-connection heuristic cannot identify the flow from an empty CID, so that path requires at least one CID byte.

**Implementation rule:** after proving mandatory framing, derive probe length from captured bytes that remain and from the minimum identity material the actual lookup needs.

**Recognition rule:** “valid packet” and “packet this heuristic can safely identify” are different sets. Reject a legal but ambiguous form if the heuristic lacks enough evidence to claim it reliably.

**Confidence:** Very high. Merged fix with direct Peter Wu review explaining the packet minimum and recognition-state requirement.
