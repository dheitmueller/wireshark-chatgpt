# Automated Wireshark MR review: !21991–!22040

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from the available per-run ledgers under `reviewed-mrs-automation/` plus `reviewed-mrs.md`, preserving and counting the historical !17571–!17620 batch. The previous automated frontier ended at !22041. No isolated reviewed MR was found in !21991–!22040, so these are the fifty highest-numbered previously unreviewed MRs in the corpus.

Reviewed count: **50**

## Exact reviewed set

- !22040 — merged — `pidl: Don't warn about params that are used` — generator bookkeeping fix; successful substitution now marks a parameter used.
- !22039 — merged release backport — `EPAN: Have proto_tree_add_bits functions set correct FI bit offset` — intra-byte bit offset must be stored modulo eight.
- !22038 — merged release backport — same EPAN bit-offset fix.
- !22037 — merged master — `Kafka: Fix decompress_snappy with no xerial chunks.` — a successful helper return must correspond to initialized/usable outputs.
- !22036 — merged master — `Take application flavor out of capture functionality` — continued migration of application policy upward; Jaap Keuter caught a lower-layer dumpcap dependency that a green pipeline had not exposed.
- !22035 — merged master — EPAN bit-offset fix; authored by Jaap Keuter and approved/merged by John Thacker.
- !22034 — merged master — `docs: Miscellaneous Developer's Guide updates` — build/platform/documentation maintenance; no new durable code convention beyond current developer guidance.
- !22033 — merged release backport — `Qt: Don't make the DataSourceTab visible if it wasn't` — temporary UI state changes must restore the original hidden state, not infer it from parent visibility.
- !22032 — merged release backport — `GitLab CI: Build the Rocky 9 packages with Qt 6` — packaging/CI maintenance.
- !22031 — closed draft, unmerged — `radiotap-gen: use libpcap from FindPCAP.cmake instead of SystemPath` — implementation down-weighted; Jaap Keuter's workflow feedback strongly corroborates use of a topic branch and enabling maintainer edits instead of opening an MR from protected `master`.
- !22030 — merged — `wsutil: Doxygen strnatcmp.h` — documentation-only.
- !22029 — merged master — `PTPV2: Malformed Packet error when parsing IEEE_C37_238 TLV` — removed duplicate parsing that advanced the offset twice and desynchronized subsequent TLVs.
- !22028 — merged master — `Qt: Don't make the DataSourceTab visible if it wasn't` — source change for the visibility-state fix later backported by !22033.
- !22027 — merged — `wsutil: Doxygen unicode-utils.h` — documentation-only.
- !22026 — merged — `wsutil: Doxygen type_util.h` — documentation-only.
- !22025 — merged — `wsutil: Doxygen to_str.h` — documentation-only.
- !22024 — merged — `wsutil: Doxygen time_util.h` — documentation-only.
- !22023 — merged — `wsutil: Doxygen tempfile.h` — documentation-only.
- !22022 — merged — `wsutil: Doxygen socket.h` — documentation-only.
- !22021 — merged — `wsutil: Doxygen sign_ext.h` — documentation-only.
- !22020 — merged — `wsutil: Doxygen rsa.h` — documentation-only.
- !22019 — merged — `wsutil: Doxygen regex.h` — documentation-only.
- !22018 — merged — `wsutil: Doxygen privileges.h` — documentation-only.
- !22017 — merged — `Doxygen please report bug.h` — documentation-only.
- !22016 — merged — `wsutil: Doxygen os_version_info.h` — documentation-only.
- !22015 — merged — `wsutil: Doxygen nstime.h` — documentation-only.
- !22014 — merged — `wsutil: Doxygen mpeg-audio.h` — documentation-only.
- !22013 — merged — `wsutil: Doxygen json_dumper.h` — documentation-only.
- !22012 — merged — `wsutil: Doxygen introspection.h` — documentation-only.
- !22011 — merged — `wsutil: Doxygen interface.h` — documentation-only.
- !22010 — merged — `wsutil: Doxygen inet_cidr.h` — documentation-only.
- !22009 — merged — `wsutil: Doxygen inet_addr.h` — documentation-only.
- !22008 — merged — `wsutil: Doxygen g711.h` — documentation-only.
- !22007 — merged — `wsutil: Doxygen filter_files.h` — documentation-only.
- !22006 — merged — `wsutil: Doxygen filesystem.h` — documentation-only.
- !22005 — merged — `wsutil: Doxygen failure_message_simple.h` — documentation-only.
- !22004 — merged — `wsutil: Doxygen clopts_common.h` — documentation-only.
- !22003 — merged — `wsutil: Doxygen buffer.h` — documentation-only.
- !22002 — merged — `wsutil: Doxygen adler32.h` — documentation-only.
- !22001 — merged — `epan: Doxygen addr_resolv.h` — documentation-only.
- !22000 — merged release backport — `GitLab CI: Fix the Debian Stable APT Test job` — CI maintenance.
- !21999 — merged master — `NetPerfMeter dissector: QUIC support` — reviewer Alexis La Goutte questioned the apparent field overlap; the contributor clarified that the byte was padding rather than flags and removed the now-obsolete `hf_addflow_flags`, reinforcing that field registrations must track actual wire ownership when formats evolve.
- !21998 — merged release-4.4 backport — `pidl: Use has_property to avoid comparing undef with the empty string` — use property-presence semantics rather than dereferencing a possibly absent Perl hash member and comparing `undef` as a string.
- !21997 — merged release-4.6 backport — same PIDL `has_property` fix.
- !21996 — merged release backport — `Netlogon: add dissection of dsraddresstositenames[ex]w` — protocol coverage/backport; no additive general convention promoted.
- !21995 — merged release-4.4 backport — `epan: Generate the filter for a FT_NONE frame bytes fallback better` — use the canonical byte-to-display-filter representation helper instead of duplicating serialization logic.
- !21994 — merged release-4.6 backport — same display-filter representation fix; John Thacker authored/merged the accepted backports.
- !21993 — merged master — `NetPerfMeter dissector improvement` — high-value review: Jaap Keuter identified TCP stream/PDU boundary handling as the root issue and explicitly directed use of `tcp_dissect_pdus()`; the revised MR handles both split PDUs and multiple PDUs per TCP segment.
- !21992 — merged release-4.6 — `GitLab CI: Fix FALCO_PLUGIN_DIR on Windows for 4.6` — corrects the release-specific dependency directory after the preceding path refactor.
- !21991 — merged release-4.6 — `GitLab CI: Set FALCO_PLUGIN_DIR relative to WIRESHARK_BASE_DIR on Win` — removes an absolute development-path assumption from CI configuration.

## Durable notebook changes

Created `transport-framing-conventions.md` with three reusable findings from this batch:

1. **TCP framing:** do not treat TCP segment boundaries as application PDU boundaries; use `tcp_dissect_pdus()` when the PDU length can be determined. This is directly supported by Jaap Keuter's review in merged !21993.
2. **Helper success contracts:** a helper returning success with output parameters must actually produce valid outputs on every successful path; merged !22037 fixes the contrary case in Kafka Snappy decompression.
3. **Canonical representation helpers:** use Wireshark's existing value/filter formatter when one exists instead of locally reproducing syntax; merged !21994/!21995 demonstrate this for byte-valued display-filter expressions.

Other findings were treated primarily as corroboration of conventions already captured elsewhere: !22036 reinforces pushing application-specific policy upward; !22031 reinforces topic-branch/maintainer-edit submission practice but is unmerged; !22029 reinforces exact parser-offset ownership; and !21999 reinforces keeping registered fields synchronized with actual wire-format semantics.

The documentation-only !22001–!22027 series was reviewed at low semantic weight: it is accepted maintenance, but it does not by itself justify manufacturing additional coding or architecture rules.
