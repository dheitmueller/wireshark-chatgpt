# Review findings !860-!909

High-confidence merged evidence:

- !897 with !901/!902: bytewise struct keys must be fully initialized, including padding bytes.
- !866/!862 and !868/!863/!861: allocator scope and release point must match the actual lifecycle and loop ownership.
- !889: typed-item checker mismatches must be resolved against the protocol specification, not mechanically.
- !871 with !895/!896: cap requested bit widths to the fixed-width integer domain before shifts and output loops; Guy Harris refined the final mask construction.
- !869 with !899/!900: malformed optional timestamp metadata should clear the timestamp-presence flag rather than make an otherwise useful record unreadable.
- !891 with !892/!893: Pascal Quantin steered a byte-aligned 32-bit little-endian field to the byte-oriented proto-tree API.
- !905: one-byte numeric key identifiers became FT_UINT8 and the extended value table was kept numerically sorted after review.
- !870: a TVBuff child must preserve the distinction between captured and reported length.
- !873: guard a late field read so a truncated packet can still be dissected as far as captured data allows.

Closed !877 and !872 were down-weighted as implementation evidence. !877 nevertheless contains useful maintainer review guidance about keeping large submissions reviewable and separating independent protocol layers.

No SMPTE ST 291/VANC packet type was encountered.
