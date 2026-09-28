# Protocol Specification Conflict Conventions
Merged MR 5965 encountered a 3GPP table entry that appeared to make NSAPI two octets. Jaap Keuter and John Thacker instead found a one-octet bit diagram, a 0-15 value range, multiple analogous one-octet NSAPI fields, compatible prose, and a matching sample capture, strongly indicating an editorial typo.

Rule: when specification artifacts conflict, triangulate bit layout, value domain, enclosing length, analogous fields, prose, revision notes, and representative captures. Document why the chosen interpretation best satisfies the coherent constraints.

Confidence: very high; merged master work with independent maintainer reasoning.
