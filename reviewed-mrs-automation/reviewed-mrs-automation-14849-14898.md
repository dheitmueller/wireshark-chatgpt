# Automated Wireshark MR review: !14849–!14898

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `1c7ad509887ee25079a7865cc62ba18cba06f49a`

Notebook base before this run: `3c16f8db8c7362bac377cf384e038ad771366eb4`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md` and all available per-run ledgers under `reviewed-mrs-automation/`, explicitly preserving/counting the historical !17571–!17620 batch. Numeric ranges were not treated as reviewed merely because neighboring MRs appeared in a ledger. With the current corpus and notebook tracking unchanged from the previous frontier, the fifty highest-numbered previously unreviewed MRs are exactly !14898 down through !14849.

Reviewed MRs (exact set, descending):

!14898, !14897, !14896, !14895, !14894, !14893, !14892, !14891, !14890, !14889,
!14888, !14887, !14886, !14885, !14884, !14883, !14882, !14881, !14880, !14879,
!14878, !14877, !14876, !14875, !14874, !14873, !14872, !14871, !14870, !14869,
!14868, !14867, !14866, !14865, !14864, !14863, !14862, !14861, !14860, !14859,
!14858, !14857, !14856, !14855, !14854, !14853, !14852, !14851, !14850, !14849.

Count: **50**.

The selected batch is merged work throughout. Merged master changes were weighted above stable-branch backports and routine automatic-update MRs. Maintainer-authored changes and substantive review comments were weighted by authority, with especially high weight given to Guy Harris's comments and authored fixes.

## Durable findings promoted

- **!14854 — merged master; high-confidence Qt performance convention.** John Thacker changed I/O Graph mouse-move handling from immediate `replot()` calls to `QCustomPlot::rpQueuedReplot`. The MR records that Linux mouse-move events may arrive about 25 microseconds apart and that queued replots dramatically improve performance. Promoted to `qt-event-coalescing-conventions.md`: high-frequency UI events should keep handlers lightweight and coalesce expensive idempotent redraw work through the event loop rather than rendering every intermediate state. Notebook commit: `5b674b46e3761e9767cc9660a97ee4034d3bcd3c`.
- **!14851 — merged master; high-confidence test-environment convention.** John Thacker updated `test_tshark_interface_chars` to accept the specific `INVALID_INTERFACE` result that occurs when Npcap is absent, in addition to the other named valid outcomes. Promoted to `test-environment-capability-conventions.md`: capability-dependent tests should enumerate the narrow semantic outcomes that are legitimate in supported environments rather than assuming all prerequisites exist or accepting arbitrary failure. Notebook commit: `2af646fb5e3636a8899fa2ee565ac91d03d1fc63`.

## Strong corroborating evidence not duplicated

- **!14895 — merged master, authored and merged by Guy Harris; extremely high weight.** `proto_tree_add_item_ret_uint()` writes a full 32-bit result, so its output argument must point to actual 32-bit storage. Casting a pointer to a smaller object merely suppresses the compiler diagnostic while allowing the helper to overwrite adjacent memory. Guy's accepted fix changes the backing variables to `guint32` and removes the casts. **!14892** independently demonstrates the same failure with a `guint8` array: `(guint32 *)&buffer_size[2]` could write beyond the array and corrupt LCID state, and Guy explicitly advised removing the casts once the backing storage was corrected. !14893 and !14896 are stable/backport companions. This strongly corroborates existing API/backing-storage and compiler-diagnostic guidance, so no duplicate rule was added.
- **!14886 — merged SocketCAN upgrade with substantive Guy Harris discussion.** Guy emphasized that several Wiretap readers normalize different source formats into `LINKTYPE_CAN_SOCKETCAN`, so dissector classification cannot assume one producer-specific padded record size. He also discussed the tension between semantic CAN-FD flags and legacy captures whose unused flag bytes may contain junk, plus the possibility that richer BLF metadata merits its own Wiretap representation. This is valuable historical architecture evidence, but later reviewed SocketCAN work supersedes the intermediate compatibility heuristic and therefore remains the stronger notebook precedent.
- **!14889 — merged master, John Thacker.** Nested dissection of packets embedded in error packets now saves and restores `pinfo->ptype`, `srcport`, and `destport` along with addresses, on both exception and normal-return paths. This directly corroborates the existing `dissector-context-flow-conventions.md` push/pop rule for temporary mutation of shared `packet_info` context.
- **!14880 — merged master.** RFC 4884's 128-byte minimum original-datagram area was enforced so zero padding is not misdecoded as multipart extensions. Alexis La Goutte requested a reproducer and the contributor supplied an ICMP/MPLS pcapng. This corroborates existing standards-driven parser-boundary and representative-capture review practice.
- **!14852 — merged master, authored and merged by Gerald Combs.** Adds the missing `decrement_dissection_depth()` in CQL. The surrounding !14849–!14850, !14861–!14864, !14867–!14878 recursion series reinforces the already-recorded requirement that recursive dissectors explicitly bound and correctly balance depth state; later project-policy evidence remains the stronger source.
- **!14859 — merged master RTCP CCFB support.** The change uses an existing RTPFB dissector table for the new feedback type while intentionally deferring migration of the older switch-based payload handlers to separate work to limit change complexity. Gerald Combs also requested ordinary C integer/size types in review. This reinforces existing extension-point, scope-control, and C-type conventions without requiring new prose.
- **!14881 — merged master, John Thacker.** I/O Graph display precision is derived from the configured interval resolution rather than generic significant-digit formatting, and an obsolete Qt workaround comment was removed because the fix predates Wireshark's minimum supported Qt. This corroborates existing presentation-precision and dependency-version guidance.
- **!14894 — merged master, John Thacker.** Replaces a fixed I/O Graph item array with `std::vector`, raises the practical capacity, and explicitly considers allocation failure. Useful resource/container evidence, but the notebook already has stronger general container and failure-path guidance.
- **!14855–!14858** are routine automatic generated-data/translation updates. **!14865/!14866** and **!14882–!14885** are CI/packaging maintenance. They were reviewed but did not justify additional durable convention text.

## Continuity

At corpus commit `1c7ad509887ee25079a7865cc62ba18cba06f49a`, `mr_14848.json` is present, so the corpus is not exhausted. Absent newly scraped higher-numbered unreviewed MRs, the next descending candidate is !14848. Future runs must still rebuild the exact reviewed set from individual tracking entries before selecting their batch.
