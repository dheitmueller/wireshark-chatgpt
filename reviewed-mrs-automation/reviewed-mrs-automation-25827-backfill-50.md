# Wireshark MR review automation ledger — 2026-09-11 backfill from !25827

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

This run reviewed exactly 50 previously unreviewed merge requests. Selection was built from the individual MR entries in `reviewed-mrs.md`, the aggregate automation ledger, and every per-run ledger in `reviewed-mrs-automation/`; numeric ranges were not assumed to be complete. The previously reviewed !17571-!17620 batch remains part of the already-reviewed set and was not revisited.

Because earlier work had already reviewed selected MRs in !25759-!25827 and every MR from !23009 upward in later contiguous ledgers, this batch is intentionally discontiguous. It reviews the remaining highest-numbered holes down through !25788 and then resumes immediately below !23009.

## Exact MRs reviewed

`!25827 !25826 !25825 !25824 !25823 !25822 !25821 !25820 !25819 !25818 !25817 !25816 !25815 !25814 !25813 !25812 !25811 !25809 !25808 !25807 !25806 !25805 !25804 !25802 !25801 !25799 !25798 !25796 !25794 !25793 !25792 !25791 !25790 !25789 !25788 !23008 !23007 !23006 !23005 !23004 !23003 !23002 !23001 !23000 !22999 !22998 !22997 !22996 !22995 !22994`

Count: **50**.

## Review notes

| MR | Status | Review result |
|---|---|---|
| !25827 | Scanned | Merged release-4.4 RDP bounds-check backport. Uses subtraction in the overflow-safe ordering (`count > limit - outputCount`) and contains no substantive human discussion beyond merge approval. Corroborates existing arithmetic-safety guidance. |
| !25826 | Scanned | Merged release-4.6 backport of the same RDP bounds fix as !25827. No additional durable lesson. |
| !25825 | Scanned | Automatic data/translation update. Merged; no durable engineering-review lesson. |
| !25824 | Scanned | Automatic data/translation update. Merged; no durable engineering-review lesson. |
| !25823 | Scanned | Automatic data/translation update. Merged; no durable engineering-review lesson. |
| !25822 | Scanned | Merged ncp2222 build fix removing a duplicate `extern` declaration from an `.inc` file after GCC 15 exposed it. Useful portability cleanup but no broader rule beyond existing header/declaration hygiene. |
| !25821 | Scanned | Stable TTL segmented-message bounds-check backport. Merged; corroborates existing captured-length and malformed-input rules. |
| !25820 | Scanned | Merged nettrace helper extraction deduplicating session-tag lookup in both reader paths. Good maintainability cleanup; no new convention beyond sharing identical parsing logic. |
| !25819 | Deep | Merged nettrace parser hardening replaces fragile raw `strstr()`/pointer arithmetic over XML with bounded libxml2 parsing, passes the available buffer length explicitly, extracts attributes through XML APIs, and frees parser resources. Strong evidence for structured parsing of structured syntax rather than reconstructing grammar with substring searches. |
| !25818 | Scanned | Merged nettrace memory-leak and `changeTime` arithmetic fix. Reinforces resource cleanup and explicit time arithmetic; no distinct new notebook rule. |
| !25817 | Scanned | Merged nettrace refactor splitting a monolithic packet-conversion routine into parsing helpers. Primarily maintainability preparation for the adjacent fixes; no independent architectural rule. |
| !25816 | Scanned | Merged TTL master bounds hardening. Later notebook entries already capture the associated wiretap error-taxonomy lesson, so no duplicate rule added. |
| !25815 | Scanned | Merged nettrace protocol-dispatch refactor replacing a hardcoded if/else chain with a table lookup. Consistent with data-driven dispatch but not strong enough to add a separate rule. |
| !25814 | Scanned | Merged C12.22 bounds-check correction. Corroborates existing defensive length-check practice. |
| !25813 | Scanned | Merged RDP master bounds fix corresponding to !25826/!25827. Corroborates existing arithmetic-safety guidance. |
| !25812 | Scanned | Merged rlogin stable backport preventing invalid UTF-8 from being placed in a column and replacing a fixed buffer with bounded allocation. Corroborates robust-text guidance. |
| !25811 | Scanned | Merged rlogin master fix corresponding to !25812. No additional review discussion; corroborates robust-text guidance. |
| !25809 | Deep | Merged release-4.6 LBM SRS fix authored by John Thacker. States the heuristic contract explicitly: heuristic dissectors must not throw for packets that simply do not belong to them. Replaces unsigned subtraction bounds logic with `tvb_captured_length_remaining()`. Promoted to `heuristic-dissector-conventions.md`. |
| !25808 | Scanned | Companion/backport of the LBM SRS heuristic exception-safety fix. Same lesson as !25809. |
| !25807 | Scanned | Companion/master variant of the LBM SRS heuristic exception-safety fix. Same lesson as !25809. |
| !25806 | Scanned | rlogin invalid-UTF-8 variant/backport. Same robust-text lesson as !25811/!25812; no additional rule. |
| !25805 | Deep | Merged John Thacker Qt cleanup uses C++14 init-capture to move a `QPointer` into a delayed lambda when supported by Qt 6.6. Correct and slightly more efficient, but intentionally treated as a local optimization rather than a repository-wide ownership mandate. |
| !25804 | Scanned | Merged Couchbase GET_EX/GET_EX_REPLICA dissection extension following existing GET opcode behavior. Protocol-specific; no durable general lesson. |
| !25802 | Scanned | libgcrypt package update variant. Dependency/package correction; no durable source-review convention. |
| !25801 | Scanned | libgcrypt package update variant. Same result as !25802. |
| !25799 | Scanned | Couchbase GET_EX/GET_EX_REPLICA variant. Protocol-specific, no new general lesson. |
| !25798 | Scanned | Merged BGP MUP SAFI draft update. Standards-version maintenance; no general coding convention. |
| !25796 | Scanned | Merged Geneve GCP timestamp option addition modeled on an existing timestamp option. Protocol-specific; no general lesson. |
| !25794 | Scanned | Merged UET TSS offset correction when an entropy header is present. Corroborates existing offset-coordinate/accounting guidance. |
| !25793 | Scanned | Merged Megaco correction from `ENC_STR_NUM` (a mask) to the actual encoding value `ENC_STR_HEX`. Reinforces using API enum/value domains correctly; no new rule needed. |
| !25792 | Scanned | Release-notes update. No durable engineering lesson. |
| !25791 | Scanned | Version bump after release build. No durable engineering lesson. |
| !25790 | Scanned | Release build/version maintenance. No durable engineering lesson. |
| !25789 | Scanned | ERF time-tag bounds-check backport/variant. Same fixed-size-copy validation already represented by previously reviewed !25786. |
| !25788 | Scanned | ERF time-tag bounds-check backport/variant. Same lesson as !25786/!25789. |
| !23008 | Discussion-focused (closed) | JSONPRO plugin submission closed unmerged with a failed pipeline. Large new XML-dictionary-driven JSON dissector; down-weighted as non-accepted architecture and not used as an exemplar. |
| !23007 | Scanned | Merged KeyboardShortcutsDialog cleanup obtains application name through application API. Consistent with frontend abstraction; no separate rule. |
| !23006 | Deep | Merged Michael Mann GUI refactor creates a Stratoshark-specific I/O Graph subclass so the shared base need not query which application is running; explicit post-construction `initialize()` enables virtual specialization. Promoted to `architecture.md`. |
| !23005 | Scanned | Merged TECMP simplification uses TVBuff remaining-length semantics directly and removes unnecessary defensive macro arithmetic. Corroborates existing TVBuff API-domain guidance. |
| !23004 | Scanned | Merged TVBuff API migration to unsigned offset/length for `tvb_get_raw_stringz`. Part of the already well-represented unsigned TVBuff domain cleanup; no duplicate rule. |
| !23003 | Discussion-focused (closed/superseded) | Earlier JSONPRO plugin submission. Down-weighted because the line was repeatedly resubmitted and remained unmerged; not an accepted architecture exemplar. |
| !23002 | Scanned | Merged dumpcap cleanup replaces magic `-1` with libpcap's `PCAP_ERROR`. Clearer API-domain expression, but no new notebook rule. |
| !23001 | Discussion-focused (closed/superseded) | Initial JSONPRO custom JSON dissector submission. Superseded by later submissions and not merged; no durable convention promoted. |
| !23000 | Scanned | Merged syslog conversion to TVBuff find helpers, fixing a quote-search bug and making an asserted invariant explicit. Corroborates existing TVBuff helper and assertion guidance. |
| !22999 | Scanned | Merged expert-info convenience APIs for highlighting the remaining TVBuff. Useful API addition; no independent architectural lesson. |
| !22998 | Scanned | Merged `tvb_raw_offset()` unsigned-domain cleanup replaces `-1` sentinel state with an explicit calculated-state flag. Strong corroboration for representing state separately instead of smuggling sentinel values into an unsigned numeric domain; existing API/domain guidance already covers this pattern. |
| !22997 | Scanned | Merged WSLua migration to `tvb_get_uint8_remaining()` plus documentation that an end-offset pointer may be NULL. Corroborates TVBuff helper/API-contract guidance. |
| !22996 | Scanned | Merged MPEG-audio parser fix advances the offset on each find-loop iteration, preventing non-progress. Corroborates existing mandatory parser-progress convention. |
| !22995 | Scanned | Merged IEC104 missing-type and endianness corrections. Protocol-specific; no general convention beyond existing wire-endianness correctness. |
| !22994 | Deep | Merged Michael Mann frontend refactor creates a Stratoshark-specific Welcome Page so the shared base does not branch on application identity. Independently corroborates !23006 and was promoted with it to `architecture.md`. |

## Durable notebook changes from this run

1. `heuristic-dissector-conventions.md`: added the no-throw contract for heuristic recognition and the requirement to use overflow-aware TVBuff remaining-length APIs rather than unsigned subtraction for probe bounds checks, based primarily on merged John Thacker !25809 and its companion variants.
2. `architecture.md`: added the repeated Wireshark/Stratoshark GUI specialization pattern from merged Michael Mann !23006 and !22994: keep the common base frontend-neutral, put application-specific behavior in subclasses, and use a post-construction initialization phase when setup requires virtual dispatch.
3. !25819 was recorded as strong additional evidence for using a real structured parser for XML rather than raw substring/pointer parsing. Existing parser/XML notes already capture the relevant trust-boundary principles, so this run did not create a redundant standalone rule for that one implementation.

## Selection continuity

After this run, the still-unreviewed frontier is **below !22994**, except for any older individual holes that future exact-ledger reconstruction discovers. Future runs must continue rebuilding the reviewed set from individual ledger entries rather than assuming the apparent numeric coverage is complete.
