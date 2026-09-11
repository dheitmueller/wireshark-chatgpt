# Dissector Registration Conventions

This file records durable guidance about automatic dissector registration and default protocol bindings.

## Do not claim an obsolete or unregistered port without enough discrimination to avoid false positives

A historical implementation using a port number is not, by itself, sufficient reason for Wireshark to claim that port automatically forever. Automatic registration should reflect a current protocol assignment or have enough additional discrimination to make the binding reliable.

Merged MR !23056, authored by John Thacker, removes the default UDP port 921 registration from the lwres dissector. lwresd had been removed from BIND years earlier, port 921 was never assigned by IANA for lwres, and the dissector did not constrain the match by loopback address or any other heuristic. Keeping the automatic binding therefore risked interpreting unrelated traffic on an unregistered port as lwres.

**Registration rule:** before adding or retaining a default port binding, verify that the port is actually assigned/authoritative for the protocol or that the dissector has another reliable discriminator. For obsolete protocols using historically conventional but unregistered ports, prefer explicit Decode As / user configuration over globally claiming the port when unrelated traffic could plausibly use it.

**Confidence:** Very high. Merged master change authored by John Thacker and approved/merged by Anders Broman.