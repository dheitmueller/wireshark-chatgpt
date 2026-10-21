# Conventions from Wireshark MRs !860-!909

- !897/!901/!902: when a struct is hashed or compared as raw bytes, initialize the complete object first; padding bytes otherwise make logically equal keys unstable.
- !866/!862 and !868/!863/!861: allocator scope must match the code lifecycle. Initialization-time data is not packet-scoped, and per-iteration owned buffers should be freed in the iteration that owns them.
- !889: resolve typed-item checker findings against the protocol specification and all field uses; do not change widths mechanically.
- !871/!895/!896: clamp requested bit counts to the width of the underlying integer before shifting or iterating.
- !869/!899/!900: if optional record metadata is malformed but payload is still usable, represent that metadata as absent rather than rejecting the record.
- !891/!892/!893: for a byte-aligned endian-coded integer, use the byte-oriented proto-tree API rather than a bit-item API.
- !905: register numeric protocol identifiers as numeric fields and keep ordered extended value tables numerically sorted.
- !870: captured and reported TVBuff lengths are separate contracts; derive the child reported length from the parent when appropriate.
- !873: guard late packet reads so snaplen-truncated packets can still be dissected as far as possible.
- Closed !877 gives useful review-process guidance but is not implementation precedent: keep large changes reviewable and keep independent protocol layers separated.

No SMPTE ST 291/VANC packet type was encountered.
