# Wireshark Address-Key Length Conventions

## Use the actual length carried by generic address objects

Wireshark's generic `address` type carries its length as part of the value. Code that turns addresses into reassembly or correlation keys should use that stored length instead of assuming a particular link-layer width.

Merged master MR !1526 fixes IEEE 1905 reassembly code that used fixed six-byte source and destination components. The accepted implementation derives its key buffer size from `key->src.len` and `key->dst.len`, copies those exact lengths, and then appends the remaining identity fields.

**Identity rule:** when a protocol key includes generic Wireshark address objects, incorporate the complete address according to its actual stored length. Do not bake a six-byte or other link-specific width into generic key construction unless the protocol contract guarantees it.

**Confidence:** Very high. Merged master correctness fix that removes a fixed-width address assumption from reassembly identity.
