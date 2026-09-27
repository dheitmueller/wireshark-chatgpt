# Wireshark Per-PDU State Conventions

Merged master MR !7861, authored by John Thacker, fixes SMTP RFC 2920/RFC 3030 pipelining. A single frame can contain several semantic units and switch between command and data state multiple times. One stored PDU type per frame is therefore insufficient. The accepted implementation stores an ordered list of per-PDU records with end offsets and parses the remaining units during the first pass.

**Rule:** when several PDUs in one frame can begin under different protocol states, persist state per PDU and retain a stable boundary such as an offset. Frame-level state is not precise enough for later redissection.

**First-pass rule:** discover and store all state transitions needed for later random-access dissection during the sequential pass; later passes should consume those records rather than infer one frame-wide state.

**Confidence:** Very high. Merged master correctness change authored by John Thacker.
