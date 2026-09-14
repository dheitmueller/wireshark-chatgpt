# Bounded Container Parser Conventions

This file records durable parsing and state-recovery conventions extracted from upstream Wireshark merge-request review. Current upstream source remains authoritative.

## Declared aggregate lengths must bound every nested item

When a wire structure begins with a length for an aggregate/list, first validate that the declared length is large enough to contain the aggregate's own mandatory header. Then treat that declared extent as a hard boundary for every nested record; do not parse items merely because the outer tvbuff still has bytes available.

Merged !20091, authored and merged by Guy Harris, fixed SMB extended-attribute list handling because the SMB_FEA_LIST size includes the size field itself. The fix rejects undersized lists, checks each nested item against the list boundary, adds corresponding SMB_GEA_LIST handling, and renames the parser for the structure it actually dissects rather than an opaque document item number.

**Implementation rule:** establish the aggregate's start/end (or remaining-byte count) once, prove its minimum representation fits, and require each child to fit within that aggregate boundary before advancing.

**Confidence:** Very high. Merged master parser fix authored and merged by Guy Harris.

## Prefer a remaining-byte invariant for variable-length record loops

For parsers that consume a bounded sequence of records, a single `remaining`/`to_read` quantity is often safer and easier to audit than repeatedly comparing absolute offsets and independently derived totals. Common padding consumption should happen in one path when all record types share it.

Merged !20115, authored and merged by Guy Harris, simplified pcapng Name Resolution Block parsing so `to_read` consistently means bytes remaining, computes record padding up front, and moves identical padding skipping outside the record-type switch.

**Implementation rule:** choose one representation for the parser boundary and update it monotonically. Avoid maintaining multiple equivalent length/offset expressions whose agreement reviewers must mentally prove.

**Confidence:** Very high. Merged master cleanup authored and merged by Guy Harris.

## Capture truncation should not unnecessarily poison later stateful dissection

For a stateful encrypted protocol, distinguish true protocol/decryption failure from capture truncation or transport fragmentation. If the cipher/framing permits recovery, preserve enough state to resume and avoid committing irreversible state transitions until the packet is sufficiently available to validate them.

Merged !20114, authored and merged by John Thacker, added encrypted SSH defragmentation and recovery across truncated packets for supported cipher modes. The change explicitly aims to allow subsequent packets to remain decryptable after a truncated frame, while using a plausibility limit for decrypted packet length and retaining state needed to continue desegmentation/decryption.

**Implementation rule:** when state advancement depends on bytes that may be absent only because of snaplen or segmentation, defer or stage that advancement until the necessary bytes are present. Do not equate a short capture with malformed protocol state when recovery is possible.

**Confidence:** Very high. Merged master stateful-dissector change authored and merged by John Thacker.

## Avoid repeat diagnostics on redissection when they describe first-pass state discovery

Diagnostics that report one-time state discovery or unsupported setup conditions should normally be emitted during the first dissection pass unless repeated output on redissection carries new information.

Merged !20117, authored and merged by John Thacker, limits the unsupported SSH KEX-type warning to the first pass among several SSH cleanups.

**Confidence:** High. Merged master cleanup by John Thacker; this also corroborates existing notebook guidance about redissection-aware state and presentation.