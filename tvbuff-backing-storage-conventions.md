# Wireshark TVBuff Backing-Storage Conventions

## A tvbuff wrapper can own object lifetime while still borrowing its backing bytes

Creating a child or composite tvbuff does not imply that the supplied real-data bytes were copied. Object-lifetime relationships and byte-storage ownership are separate contracts; freeing or reusing the backing allocation while a tvbuff still references it produces a use-after-free even if the tvbuff itself is correctly attached to a parent.

Merged master MR !10823, authored and merged by John Thacker, fixes Kafka decompression with the explicit observation that neither `tvb_new_child_real_data()` nor `tvb_composite_append()` copies the real-data buffer it is given. The accepted code allocates decompressed bytes in packet scope, shrinks the buffer before wrapping it when appropriate, and no longer frees each chunk after appending its child tvbuff. Stable-branch counterparts !10825 and !10826 carry the same fix.

**Implementation rule:** before passing real-data storage into a tvbuff API, determine whether that API copies, takes ownership, or merely references the bytes. If it references them, finalize any realloc/shrink operation before wrapping and keep the backing allocation alive for every downstream tvbuff/composite consumer. Parent/child tvbuff ownership solves tvbuff-object cleanup; it does not by itself license early release of borrowed byte storage.

**Confidence:** Very high. Merged master memory-safety fix authored and merged by John Thacker, independently propagated to two release branches with the ownership contract documented in the MR itself.
