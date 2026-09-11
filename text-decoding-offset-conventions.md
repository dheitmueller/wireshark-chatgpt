# Wireshark Text-Decoding and Offset Conventions

This file records durable conventions for preserving byte-offset semantics while decoding textual protocol data. Current upstream source remains authoritative.

## Do not recover wire offsets by searching decoded text

Decoding can change both byte width and content, so a character/string position is not a reliable coordinate in the original tvbuff. Do not extract a packet-controlled string, search that decoded representation for delimiters or substrings, and then translate the result back into an original-frame byte offset unless the encoding contract proves the mapping is one-to-one.

Merged master MR !23337, authored by John Thacker and merged by Anders Broman, removed broken SRVLOC handling that decoded UCS-2/text and then searched the resulting string to infer offsets. The MR explicitly notes that this fails with fuzzed data, and the diff explains that arbitrary or variable-width encodings make decoded-string positions difficult to match to original frame locations.

**Implementation rule:** keep parsing coordinates in the buffer whose bytes are actually being parsed. For variable-width, escaped, or normalized text, either parse delimiters and lengths directly from the original tvb with encoding-aware APIs, or materialize the decoded representation as a derived tvb/data source and parse relative to that derived source. Tree-item offsets must belong to the same data source whose bytes they describe; do not rediscover original packet positions by substring search after decoding.

**Confidence:** Very high. Merged fuzzing/correctness fix authored by John Thacker with an explicit explanation of the offset/encoding failure mode.
