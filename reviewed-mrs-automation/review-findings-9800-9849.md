# Review findings: !9800-!9849

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs in this batch are merged. Merged master changes and direct maintainer review were weighted most strongly; stable backports mainly corroborate their master counterparts.

## Strongest durable findings

- **!9830, John Thacker:** RTPS now grows its element collection as entries are parsed instead of sizing persistent storage from the full packet-declared count at the start. This is strong evidence for matching storage commitment to parsing progress.
- **!9822, !9836, !9844:** this sequence records a cross-version Qt/C++ portability correction. The final form uses a range-based loop compatible with the project's supported language and Qt versions.
- **!9819 and !9840, John Thacker with Roland Knall review:** keep model APIs in their semantic domain and translate source/proxy/display column coordinates at the layer that owns the proxy. Handle an empty proxy model explicitly and avoid repeated mapping work for every row.
- **!9817:** John Thacker's MySQL review shows that the same leading value can have different meanings in different protocol states, so the current transaction state must help select the grammar.
- **!9801, John Thacker:** mixed repeatable TShark field-selection options require per-entry mode state rather than one global mode shared by the whole list.
- **!9837, Martin Mathieson, with Guy Harris review:** value-table checks should prompt semantic review of the field width and lookup domain. !9845 documents an intentional transformed lookup domain as an exception rather than hiding it.
- **!9846, Guy Harris:** a value carried from matching-request state into a response should not claim unrelated response bytes as its source, and the registered field width must match the semantic value.
- **!9839:** Alexis La Goutte requested a release-note entry and representative capture for the new TRDP dissector; both were supplied. Martin Mathieson separately noted the framing work required before adding TCP support.
- **!9831:** Jaap Keuter's logging documentation, refined by João Valverde, separates logging domain, level, and output channel and records the relevant developer-facing behavior.
- **!9838:** Stig Bjørlykke's review favors one shared decoding path and field namespace for identical BBLog metadata once common block/option dispatch can support it.

The remaining MRs were reviewed for purpose, outcome, discussion, and relevant diffs. They mainly consisted of protocol-local updates, stable backports, automated registry data, documentation corrections, and corroborating fixes to existing notebook rules. No SMPTE ST 291/VANC packet type was encountered.
