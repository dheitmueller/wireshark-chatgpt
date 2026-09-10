# Wireshark Parser Boundary Conventions

This file records durable conventions for protocol length, extent, and encoding boundaries extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Validate payload boundaries against the header extent actually parsed

When a protocol header can vary in size, payload-presence and length validation must use the effective header extent computed from the packet, not the protocol's minimum or most common header size.

Merged MR !25250 fixes IPv4 empty-payload detection so it compares the total IP length to the parsed `hlen`. The earlier fixed-size comparison misclassified packets carrying IPv4 options because their valid header is larger than the minimum header.

**Implementation rule:** once parsing has established a variable header/substructure extent, use that value for later payload-boundary tests, remaining-length calculations, and validity checks. Do not reintroduce a fixed nominal size downstream.

**Confidence:** Very high. Merged master parser correction with direct Jaap Keuter review identifying the variable-header issue.

## Verify the specification's field encoding, not only values seen in sample traffic

A parser can appear correct while test values remain below an encoding boundary. When a specification uses a wider fixed field, variable-length quantity, or endian-specific representation, model the specified encoding directly rather than choosing a narrower helper because current samples happen to fit.

Merged MR !25230 corrects BSSMAP LE parsing after a two-byte length field had been treated as a variable-length quantity; the distinction becomes visible for values at and above the high-bit boundary. The merged correction follows the defined wire representation rather than the behavior of small examples.

**Implementation rule:** review field width and encoding against the normative format and include boundary values that distinguish plausible alternative interpretations. Small-value captures are insufficient evidence for choosing a narrower or variable-length decoder.

**Confidence:** Very high. Merged master protocol-correctness fix authored by John Thacker.

## Keep malformed length-delimited elements synchronized to their declared boundary

When a container is a sequence of independently length-delimited elements, a malformed element should not normally cause its decoder to drift into the next element. Validate the fixed header before reading it, compute the element's declared end once, constrain key/type-specific parsing to that extent, report semantic length/value violations with Expert Info, and resume at the declared element boundary when doing so is safe.

Merged MR !24942 applies this pattern to DNS SVCB/HTTPS `SvcParam` parsing. It first rejects a truncated parameter header, calculates `param_end` from the declared parameter length, adds key-specific checks for parameters such as `mandatory`, `port`, IPv4/IPv6 hints, ALPN, and zero-length flags, and then explicitly resynchronizes so one malformed parameter does not misalign all following parameters. The MR included a crafted malformed capture and was approved/merged by Alexis La Goutte.

**Implementation rule:** distinguish "this element is malformed" from "the enclosing sequence can no longer be located." If the outer framing still provides a trustworthy declared boundary, keep all reads inside it and resume from that boundary after reporting the defect rather than letting inner parsing consume bytes belonging to the next element.

**Confidence:** Very high. Merged master parser-hardening change with explicit malformed-input validation and a reproducer.