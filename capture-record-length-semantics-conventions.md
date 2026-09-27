# Capture Record Length Semantics

Merged MR !7057 removes Wiretap's generic clamping of captured length to packet length. Tomasz Moń showed that the clamp could discard bytes actually present in a capture. Guy Harris explained the pcap/pcapng model: captured length is the amount retained in the record, while packet length is the unsliced logical record size.

**Rule:** generic capture-reader code should preserve bytes that are actually present. If a record has inconsistent length metadata, diagnose or repair it in the format/linktype-specific layer where enough semantics exist to do so. Avoid silently converting producer corruption into apparent snapshot truncation.

**Confidence:** Extremely high. Merged change with extensive direct Guy Harris review.
