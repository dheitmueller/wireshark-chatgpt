# Wireshark Nested Container Boundary Conventions

Merged master MR !5840, with stable backports !5847 and !5848, bounds PROXY v2 TLV parsing by the declared PROXY header end rather than by the remaining packet bytes. The durable convention is that nested parsers use the enclosing semantic container boundary; packet end is not a substitute for a narrower declared structure boundary.
