# Review findings: !9850-!9899

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

The batch contains 48 merged MRs and two closed/unmerged MRs (!9859 and !9852). Merged work is weighted more strongly; closed/superseded work is retained only as lower-weight history.

## Strongest durable findings

- **!9894 + !9895, John Thacker:** dependent-frame bookkeeping is dissection-derived state. Clear it when frame data resets, avoid duplicate recursive marking, and model the collection as a set/hash table when order is irrelevant. This improves redissection correctness and large-reassembly performance.
- **!9882 + !9887:** Gilbert Ramirez explicitly recommended changing a TECMP voltage from text to `FT_DOUBLE` so numeric display filters work; merged master !9887 implements that with units.
- **!9889, John Thacker, reviewed by Stig Bjørlykke:** report bad coloring-rule syntax when it is loaded/profile-switched, with the actual parse failure, rather than delaying a generic warning until the editor is opened. Consolidate multiple warnings.
- **!9877:** finite protocol identifiers can wrap; PTP sequence matching now combines the identifier with a time plausibility bound rather than assuming the 16-bit value is unique forever.
- **!9875, Alexis La Goutte review:** use `proto_tree_add_item_ret_uint()` when a parsed value is both displayed and consumed; use `NULL` instead of an empty blurb; expose reserved/unused flag bits; include a representative pcap.
- **!9865, multi-maintainer review:** field names should track specification terminology, flag naming should be consistent, non-generated fields should not use generated-style bracket presentation, unimplemented payload should remain visible via data/opaque bytes, named bit masks are preferable to magic numbers, and a new dissector should include a representative capture and release-note entry. With no assigned fixed port, Decode As is preferable to inventing one.
- **!9861 + !9862, Guy Harris:** a response field copied from request state must not claim unrelated response bytes; use no source range for those bytes and register the field at its actual semantic width.
- **!9860 + !9858, Guy Harris:** broad field-width/`value_string` corrections strongly corroborate typed-field/checker domain rules.
- **!9856:** broad migration from ambient `wmem_packet_scope()` to explicit `pinfo->pool` corroborates allocator-context/lifetime guidance.
- **!9850:** TLS/QUIC GREASE recognition is centralized in exact semantic predicates; the prior approximate TLS test would falsely classify a near-miss such as 0x1a2a.

## Per-MR inventory

| MR | Outcome | Review note |
|---|---|---|
| !9899 | merged release-3.6 | John Thacker NR-RRC assignment-vs-comparison fix; backport of !9893. |
| !9898 | merged release-4.0 | Same NR-RRC correctness backport. |
| !9897 | merged master | Guards zero-length UDS byte-string formatting helpers. |
| !9896 | merged master | O-RAN FH CUS extension 2; protocol-local feature. |
| !9895 | merged release-4.0 | John Thacker replaces dependent-frame list with hash/set semantics for scale. |
| !9894 | merged release-4.0 | John Thacker deduplicates dependencies and clears them on frame-data reset. |
| !9893 | merged master | John Thacker fixes `value=0` vs `value==0` in ASN.1 source and generated C. |
| !9892 | merged master | John Thacker applies item-length fixes to ASN.1 templates so regeneration preserves them. |
| !9891 | merged master | WSLua errors gain a traceback subtree. |
| !9890 | merged master | O-RAN extensions; MR explicitly says “Untested,” so weak test precedent. |
| !9889 | merged master | Actionable coloring-rule parse errors are reported at load time. |
| !9888 | merged master | John Thacker fixes coloring-rule clone lifetime leak. |
| !9887 | merged master | TECMP voltage becomes numeric `FT_DOUBLE` with units. |
| !9886 | merged master | O-RAN ext20; uses return-value tree helper. |
| !9885 | merged release-4.0 | UDS RDTCI value/name correction backport. |
| !9884 | merged master | UDS RDTCI identifiers 0x0b-0x0e corrected. |
| !9883 | merged master | John Thacker uses Wireshark bit-rate formatter because Qt formatter is byte-oriented. |
| !9882 | merged release-3.6 | Gilbert Ramirez proposes numeric voltage semantics; master follow-up implements it. |
| !9881 | merged master | Martin Mathieson typed-item checker cleanup fixes real widths/masks/fetch widths. |
| !9880 | merged master | Logo/Inkscape-SVG rendering discussion; resource-specific. |
| !9879 | merged release-4.0 | TECMP formatting backport. |
| !9878 | merged master | Log-domain tokenization simplification; empty-token behavior explicitly reviewed. |
| !9877 | merged master | PTP sequence wrap disambiguated with time context. |
| !9876 | merged master | TECMP textual voltage formatting fix, superseded semantically by !9887. |
| !9875 | merged master | RPKI-RTR ASPA with sample capture and substantial Alexis La Goutte review. |
| !9874 | merged master | John Thacker wmem/build-type documentation. |
| !9873 | merged master | Sharkd `prev_frame`/`ref_frame` schema types corrected to unsigned integer. |
| !9872 | merged master | Clang Analyzer dead-store cleanup. |
| !9871 | merged master | Filter namespace corrections after conflict. |
| !9870 | merged master | Capture-relative timestamp propagated from Wiretap and exposed as generated frame metadata. |
| !9869 | merged master | Wi-SUN helper returns correct consumed length/offset. |
| !9868 | merged master | Conversation UI includes date when capture duration exceeds 24h. |
| !9867 | merged master | Logging developer documentation cleanup. |
| !9866 | merged master | Dissector skeleton updated for current logging/header conventions. |
| !9865 | merged master | Initial Matter dissector; deep multi-maintainer review and representative pcap. |
| !9864 | merged release-3.6 | Guy Harris documents GMR-1 ambiguous type mapping/historical 0x100 hack. |
| !9863 | merged release-4.0 | Same high-authority GMR-1 documentation backport. |
| !9862 | merged release-3.6 | Guy Harris request-derived Gryphon response value gets no false byte range and correct width. |
| !9861 | merged release-4.0 | Same Guy Harris provenance/field-width correction. |
| !9860 | merged release-3.6 | Guy Harris field-width/`value_string` correctness backport. |
| !9859 | closed/superseded | Failed first backport attempt; replaced by merged !9860. |
| !9858 | merged release-4.0 | Guy Harris field-width/`value_string` correctness backport. |
| !9857 | merged master | Decode As list uses human/locale-aware case-insensitive ordering. |
| !9856 | merged master | Explicit `pinfo->pool` allocator context propagated through many helpers. |
| !9855 | merged master | Logging documentation grammar only. |
| !9854 | merged master | RTPS support-query dissection improvements. |
| !9853 | merged master | Time Display Format tooltip correction. |
| !9852 | closed/superseded | Earlier RTPS submission; superseded by !9854. |
| !9851 | merged master | BBLog PRU event support. |
| !9850 | merged master | Exact named TLS/QUIC GREASE predicates replace loose repeated tests. |

## Notebook promotion

New focused convention files from this run:
- `dependent-frame-state-conventions.md`
- `protocol-value-provenance-conventions.md`
- `numeric-field-semantics-conventions.md`
- `reserved-value-recognition-conventions.md`
- `undecoded-payload-conventions.md`

!9892/!9893 strongly corroborate the existing generated-code source-of-truth rule; !9881/!9860/!9858 corroborate existing typed-field checker guidance; !9875/!9865 corroborate existing representative-capture expectations. Those were retained here rather than duplicating already-established topical rules.

No SMPTE ST 291/VANC packet type was encountered.
