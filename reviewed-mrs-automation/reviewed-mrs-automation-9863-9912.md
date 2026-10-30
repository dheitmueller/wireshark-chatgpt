# Wireshark MR review ledger — !9912 through !9863

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `42f39fdd4d7a165d293fce654fcd82e463d62e6e`
Review date: 2026-10-30
Review direction: newest available unreviewed MR toward older MRs.
**Exactly 50 distinct MRs reviewed: 49 merged; one closed without merging (!9907).**

## Selection and duplicate exclusion

The corpus commit above contains 26,575 distinct MR JSON files, and all fifty `mr_9912.json` through `mr_9863.json` exist. Before selecting, inventoried all 348 paths in `reviewed-mrs-automation/`, consulted `reviewed-mrs.md` and the supplemental automation tracking, examined the latest two exact ledgers, and checked exceptional/noncontiguous/gap ledger contents. Last run's exact list ends at !9913; the aggregate ledgers and searched relevant files provided no previous review evidence for any candidate below !9913. Higher numbered MRs already recorded in exact tracking were excluded by membership, not by assuming that every integer between ledger endpoints is reviewed. Only the present fifty corpus MRs were newly examined in this run.

The preserved `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` was independently parsed: its table has **50 distinct rows covering every MR !17571–!17620**, with none missing or extra. That prior review remains part of the historical reviewed set; its file was not modified.

MR discussions, author/merge outcomes, change files and substantive code diffs were examined from this corpus snapshot. Merged code and review-driven revisions were weighted above abandoned proposals; direct maintainer guidance was weighted explicitly. !9907 remains useful as a **closed/rejected stable-branch policy example**, never as accepted code precedent. Some older MRs are cherry-picks of the same change, so they are corroboration rather than independent design decisions.

## Exact reviewed MR numbers (descending)

!9912, !9911, !9910, !9909, !9908, !9907, !9906, !9905, !9904, !9903, !9902, !9901, !9900, !9899, !9898, !9897, !9896, !9895, !9894, !9893, !9892, !9891, !9890, !9889, !9888, !9887, !9886, !9885, !9884, !9883, !9882, !9881, !9880, !9879, !9878, !9877, !9876, !9875, !9874, !9873, !9872, !9871, !9870, !9869, !9868, !9867, !9866, !9865, !9864, !9863

## Individual review findings

| MR | Outcome | Depth | Finding |
| --- | --- | --- | --- |
| !9912 | merged | Discussion-focused | Gilbert Ramirez requested matching Logray selected-field menu cleanup; merged diff updates Wireshark and Logray. |
| !9911 | merged | Scanned | UDS removes unused/nonstandard service subtree levels and ETT registrations. |
| !9910 | merged | Scanned | Release-4.0 backport of !9905: skip empty WDBI payload format/subdissection. |
| !9909 | merged | Scanned | Master weekly generated registry/translations/manuf/PCI ID update. |
| !9908 | merged | Scanned | Release-3.6 weekly automatic registry update corresponding to master. |
| !9907 | closed/unmerged | Discussion-focused | Closed: Jaap Keuter rejected ASTERIX feature update on stable branch; Gerald Combs fixed updater; superseded by !9915. |
| !9906 | merged | Discussion-focused | UDS obsolete parser helpers removed; Martin Mathieson review led to splitting terminology typo into separately backportable !9952. |
| !9905 | merged | Deep | UDS WDBI zero-record guards prevent TVB subset and formatting assertions; !9910 backport. |
| !9904 | merged | Deep | Nested reassembly dependencies persist on frame_data; recursively mark ancestors and use hash sets for fast de-duplication. |
| !9903 | merged | Scanned | Remove unused Qt PacketListModel GStringChunk member. |
| !9902 | merged | Scanned | RTP Analyze/Play QActions parented to their QToolButtons to prevent leaks. |
| !9901 | merged | Deep | Qt Export Packet Bytes crash workaround uses QueuedConnection; Gerald Combs and John Thacker insist on Logray parity. |
| !9900 | merged | Scanned | Release-4.0 backport of !9897 UDS nonempty byte-format guards. |
| !9899 | merged | Scanned | Release-3.6 NR-RRC ASN.1 CNF plus generated-code comparison fix from !9893. |
| !9898 | merged | Scanned | Release-4.0 NR-RRC CNF plus generated-code comparison fix from !9893. |
| !9897 | merged | Deep | Zero-length UDS records must not call tvb_bytes_to_str_punct; header/context remains displayable. |
| !9896 | merged | Scanned | O-RAN FH CUS section extension type 2 implementation. |
| !9895 | merged | Deep | John Thacker replaces unordered dependent-frame GSList with GHashTable; reset/destroy lifecycle handled. |
| !9894 | merged | Deep | John Thacker suppresses duplicate dependency inserts and clears persisted dependencies on frame reset. |
| !9893 | merged | Deep | John Thacker fixes ASN.1 value=0 vs value==0 in NR-RRC CNF and generated dissector; Coverity. |
| !9892 | merged | Deep | John Thacker repairs ASN.1 ANSI MAP/TETRA canonical templates so generated fixes survive regeneration. |
| !9891 | merged | Deep | WSLua lua_pcall error handler adds expert error and per-line Lua Traceback tree with stack discipline. |
| !9890 | merged | Scanned | O-RAN section extensions 16/17/18/21; author marks untested, so no strong test practice inference. |
| !9889 | merged | Discussion-focused | Stig Bjørlykke approved immediate detailed color-filter errors on profile load despite popup tradeoff. |
| !9888 | merged | Scanned | Free cloned color_filter_t after the ColoringRuleItem copies needed strings. |
| !9887 | merged | Deep | TECMP voltage field converted from FT_STRING to filterable FT_DOUBLE after Gilbert Ramirez review. |
| !9886 | merged | Scanned | O-RAN section extension 20; marked untested by author. |
| !9885 | merged | Scanned | Release-4.0 backport of UDS RDTCI subfunction naming corrections. |
| !9884 | merged | Scanned | Correct UDS RDTCI 0x0b through 0x0e standard subfunction assignments. |
| !9883 | merged | Scanned | Use wsutil format_size with bits-per-second units, not Qt data-size byte units. |
| !9882 | merged | Discussion-focused | Gilbert Ramirez explicitly requests filterable numeric voltage field; accepted master successor !9887. |
| !9881 | merged | Scanned | Typed protocol-item field-width/mask corrections discovered by check_typed_item_calls.py. |
| !9880 | merged | Discussion-focused | Jaap, Gilbert, Gerald compare Inkscape SVG against browser rendering; source editability versus plain SVG portability. |
| !9879 | merged | Scanned | Release-4.0 TECMP voltage two-decimal-digit display correction. |
| !9878 | merged | Discussion-focused | Jaap Keuter probes empty tokens after g_strsplit_set refactor; accepted with inefficiency caveat. |
| !9877 | merged | Deep | PTP long-trace seqid rollover requires timestamp-window disambiguation and redissection-stable match metadata. |
| !9876 | merged | Scanned | Master TECMP voltage formatting uses two decimal places; !9879/!9882 backports. |
| !9875 | merged | Deep | Alexis La Goutte: avoid unnecessary TVB alignment, use add_item_ret_uint, show reserved bits; sample pcap supplied. |
| !9874 | merged | Discussion-focused | John Thacker and Jaap Keuter align wmem/build-type documentation with Developer Guide structure. |
| !9873 | merged | Scanned | Sharkd frame RPC prev_frame/ref_frame should be unsigned integer parameters, not booleans. |
| !9872 | merged | Scanned | Remove dead-store offset update from Darwin pcapng option parsing. |
| !9871 | merged | Scanned | Repair RTPS registered-field namespace and SHICP expert-info prefix. |
| !9870 | merged | Scanned | Introduce validity-guarded capture-start-relative timestamp from Wiretap through EPAN to packet-frame field. |
| !9869 | merged | Scanned | Wi-SUN LBS-IE dissector must return actual 7-byte consumption, not 6. |
| !9868 | merged | Scanned | Qt Conversations display date for captures over 24 hours and accept date/time filter values. |
| !9867 | merged | Scanned | Developer Guide logging explanation/doc wording maintenance. |
| !9866 | merged | Scanned | Refresh dissector skeleton with config.h, WS_LOG_DOMAIN, wireshark.h includes. |
| !9865 | merged | Deep | Matter: spec-first filter names, named bit masks, honest opaque payloads, sample pcap, release note and squash. |
| !9864 | merged | Discussion-focused | Guy Harris documents conflicting ETSI GMR-1 codes and the historical synthetic 0x100 workaround; release-3.6. |
| !9863 | merged | Discussion-focused | Guy Harris's same GMR-1 standards ambiguity explanation on release-4.0. |

## Durable notebook updates

- `reassembly-conventions.md`: transitive, persistent frame dependency graphs and set-based deduplication (!9904, !9894, !9895).
- `protocol-identity-key-conventions.md`: temporal disambiguation of wrapping PTP sequence numbers (!9877).
- `dissector-naming-conventions.md`, `dissector-conventions.md`: spec-first Matter naming, explicit partial/opaque payloads, RPKI field extraction and reserved flags, and Guy Harris's documented GMR-1 standard ambiguity (!9865, !9875, !9864, !9863).
- `protocol-field-semantic-conventions.md`: numeric filterability of measured TECMP voltage (!9887 and !9882).
- `qt-signal-connection-conventions.md`, `qt-object-parenting-conventions.md`: queued Qt action connections and GUI parity; action parenting (!9901, !9912, !9902).
- `generated-code-conventions.md`: canonical ASN.1 CNF/templates corrected alongside generated output (!9892, !9893).
- `wslua-conventions.md`: Lua traceback diagnostics and protected-call stack discipline (!9891).
- `submission-conventions.md`: new dissector submission completeness and stable-branch automated-update restriction (!9865, !9907).
- `parser-edge-cases.md`: empty-payload TVB helper guards (!9905, !9897).
- `capture-start-time-conventions.md`: record-backed capture-start-relative time distinct from first-frame relative (!9870).

**Next expected candidate:** !9862, subject to checking for any newly scraped higher-numbered unreviewed MRs in the corpus on the next run.
