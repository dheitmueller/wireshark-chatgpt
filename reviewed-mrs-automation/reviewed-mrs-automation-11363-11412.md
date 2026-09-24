# Wireshark MR automation review: !11363-!11412

Reviewed against corpus repository `dheitmueller/wireshark-corpus-mrs` at commit `ddcaa22b51c68f594e425a23388c3a2086813054`.

Before selecting this run, the reviewed set was reconstructed from the available `reviewed-mrs-automation/` per-run ledgers and `reviewed-mrs.md`. The historical !17571-!17620 batch remains part of the reviewed set. The immediately preceding authoritative run ledger covers !11413-!11462. Selection was made by individual MR number, not by assuming that any numeric interval was wholly reviewed. No existing review record was found for the selected MRs below. The previous mention of !11412 was only a frontier probe and did not count as a review.

This run reviewed exactly 50 MRs: !11412 down through !11363. All 50 are merged in the corpus snapshot, so no closed/abandoned MR required reduced implementation weight. Low-information release, automatic-data, and backport MRs were scanned rather than over-weighted; merged master fixes and substantive maintainer/author reasoning received deeper treatment.

Notebook changes produced by this run:

- `callback-context-conventions.md`: merged !11378 and follow-up !11410 establish that plugin callbacks should receive explicit typed context for negotiated runtime metadata while keeping plugin-private mutable state separately owned. Commit `e71793878cc5c0aefa8ac7540bd4a67cc6152cb5`.
- `buffer-length-return-conventions.md`: merged !11382 establishes that a representation-changing conversion invalidates assumptions that the source byte length is also the output byte length; downstream length must be measured in the output representation. Commit `9768c4763cf772d9c2b03047eecd008a9b58f65c`.
- `order-independent-metadata-conventions.md`: merged !11392 establishes find-or-create/merge state for metadata records that may legally arrive in either order, using SDP `fmtp` before `rtpmap` as the concrete case. Commit `d846a2f285f0ab788182688aaea3ed57b72a7879`.

## Exact reviewed set

| MR | Review status | Notes |
|---|---|---|
| !11412 | Scanned, merged | DHCPFO vendor-specific option handling generalized beyond Microsoft; vendor-qualified field names and raw display of unknown option data. Merged by John Thacker; no substantive human-review correction in the snapshot. |
| !11411 | Deep, merged | John Thacker-authored T.38 fix suppresses false malformed diagnostics for retransmitted fragment ends whose data was already reassembled on the previous packet. Strong corroboration for retransmission-aware state/diagnostic logic. |
| !11410 | Deep, merged | John Thacker-authored codec follow-up passes negotiated `fmtp` data into codec context and uses it to distinguish AMR octet-aligned vs bandwidth-efficient framing. Promoted with !11378 to callback-context guidance. |
| !11409 | Scanned, merged | Gerald Combs adds a CBOR recursion check. Corroborates the existing bounded-recursion convention; no duplicate notebook rule added. |
| !11408 | Deep, merged | Long-lived DTLS 1.3 implementation. Correct version must come from handshake `supported_versions`, not legacy record-version bytes; adds compact-header/ACK support and evolved through analyzer/test fixes before eventual merge. Merged implementation weighted more heavily than its early draft state. |
| !11407 | Scanned, merged | Debian lintian override maintenance. Packaging-specific; no durable engineering rule beyond existing packaging/build guidance. |
| !11406 | Scanned, merged | Release-4.0 CFM backport/workaround for the TEST-ID TLV specification contradiction. Treats this specific TLV as four value octets despite the standardized length value of 32. Corroborates specification-exception handling. |
| !11405 | Scanned, merged | Release version bump 3.6.15 to 3.6.16; maintenance-only. |
| !11404 | Scanned, merged | Release version bump 4.0.7 to 4.0.8; maintenance-only. |
| !11403 | Discussion-focused, merged | TLS explicitly labels an incomplete trailing segment rather than silently claiming otherwise-undissected bytes; analogous to !11399. Useful presentation/partial-data behavior, but no new convention needed. |
| !11402 | Scanned, merged | Build/release metadata for 3.6.15; maintenance-only. |
| !11401 | Scanned, merged | Build/release metadata for 4.0.7; maintenance-only. |
| !11400 | Deep, merged | John Thacker-authored IPP/IPPS support exposes an important dispatch-context issue: HTTP reached heuristically cannot assume `pinfo->match_uint` contains the transport port, so request/response direction is used to recover the applicable endpoint port. Strong corroboration for existing dissector dispatch-context rules. |
| !11399 | Scanned, merged | DTLS labels unknown record data explicitly and adds the DTLS 1.3 version value. Corroborates preserving visibility of undecoded bytes. |
| !11398 | Scanned, merged | User Guide preference documentation; documentation-only. |
| !11397 | Scanned, merged | Adds Aruba UBT dissector/protocol support. Accepted feature with no additional high-value human-review lesson identified in this snapshot. |
| !11396 | Discussion-focused, merged | USB Darwin status update distinguishes overall request status from per-isochronous-frame status; avoids copying aggregate status into each frame where that would falsely imply identical per-frame outcomes. |
| !11395 | Deep, merged | John Thacker-authored RTP analysis fix uses signed timestamp-difference semantics because reordered/earlier packets can make the expected arrival delta negative. Corroborates signed/wraparound arithmetic discipline. |
| !11394 | Scanned, merged | Release preparation for 3.6.15; maintenance-only. |
| !11393 | Scanned, merged | Release preparation for 4.0.7; maintenance-only. |
| !11392 | Deep, merged | John Thacker-authored RTP fix preserves SDP `fmtp` that appears before `rtpmap` by creating a payload placeholder and later enriching the same keyed object. Promoted to `order-independent-metadata-conventions.md`. |
| !11391 | Scanned, merged | Removes an empty services file accidentally produced by an automatic update. Automation cleanup; no new convention. |
| !11390 | Deep, merged | Master CFM TEST-ID handling recognizes a standards contradiction: ITU-T specifies length 32 for a four-octet value despite the IEEE generic TLV definition using octets. Accepted code applies a type-specific interpretation and annotates it rather than globally changing TLV semantics. |
| !11389 | Scanned, merged | Minor `check_typed_item_calls.py` fixes. Corroborates continued investment in source-level dissector validation; no separate new rule. |
| !11388 | Scanned, merged | Makes `wmem_map.h`/`wmem_multimap.h` include the header defining `wmem_list_t`, which their public APIs return. Corroborates self-contained public-header dependency hygiene. |
| !11387 | Deep, merged | Replaces runtime parsing of generated `manuf` text with generated sorted static arrays and binary search. Martin Mathieson review also called out opaque discriminator readability and explicitly checked whether a short address buffer was copied. Corroborates existing generated-data/startup and ownership-review guidance. |
| !11386 | Scanned, merged | Automatic data/translation update; services generation failed. Process/data maintenance only. |
| !11385 | Scanned, merged | Automatic data/translation update; services generation failed. Process/data maintenance only. |
| !11384 | Scanned, merged | Improves BT-DHT Info-column packet type/parameter presentation. User-facing presentation improvement; no durable architecture rule extracted. |
| !11383 | Deep, merged | John Thacker-authored HTTP fix warns when bytes remain after a declared Content-Length and do not look like a valid pipelined message, treating the mismatch as evidence of a likely bogus length rather than silently consuming it. Corroborates bounded-message validation. |
| !11382 | Deep, merged | WSLua string conversion recalculates length after `tvb_get_string_enc()` instead of using the source TVB range length. Promoted to post-conversion/output-length guidance in `buffer-length-return-conventions.md`. |
| !11381 | Scanned, merged | Automatic data/translation update. No additional durable rule. |
| !11380 | Scanned, merged | Keeps a global enterprises file as an optional system-wide override even after bundled external data is removed. Compatibility/configuration-path maintenance. |
| !11379 | Scanned, merged | Keeps a global services file optional for administrators/legacy users although Wireshark no longer ships one. Compatibility/configuration-path maintenance. |
| !11378 | Deep, merged | John Thacker-authored codec API redesign carries negotiated RTP/SDP sample rate and channel count in a typed `codec_context_t` while keeping codec-private state in `priv`. Promoted to callback-context guidance. |
| !11377 | Discussion-focused, merged | Removes obsolete Cavebear OUI data and relies on the official IEEE registry. Corroborates using an authoritative generated source rather than maintaining stale duplicate registry data. |
| !11376 | Scanned, merged | DCERPC WKSSVC request/response parsing fix supplied with a concrete failure capture and before/after evidence. Corroborates representative-capture validation. |
| !11375 | Scanned, merged | Propagates SDP audio-channel information into RTP dynamic-payload state; part of the accepted negotiated-codec context sequence. |
| !11374 | Scanned, merged | DCERPC SRVSVC request/response parsing fix supplied with the same focused failure capture and before/after evidence. Corroborates capture-backed validation. |
| !11373 | Scanned, merged | Parses optional SDP `rtpmap` audio-channel metadata and carries it in transport state for downstream use. Foundation for !11375/!11378. |
| !11372 | Scanned, merged | Adds display-filter XOR logical operator. Language feature; no separate convention extracted. |
| !11371 | Scanned, merged | Release-note item movement; documentation/release maintenance only. |
| !11370 | Deep, merged | John Thacker corrects a prior COPS leak fix after recognizing that `wmem_array_index()` returns a pointer to the stored element; when the stored element is itself a pointer, callers receive a pointer-to-pointer. Corroborates reading container element/accessor contracts literally instead of relying on type intuition. |
| !11369 | Scanned, merged | Backport of AMQP NULL formatter-length fix. Same implementation lesson as master !11366, no additional evidence. |
| !11368 | Scanned, merged | Second stable-branch backport of AMQP NULL formatter-length fix. No additional lesson beyond !11366. |
| !11367 | Scanned, merged | Adds Kerberos pre-authentication registry values. Registry/data update only. |
| !11366 | Discussion-focused, merged | Master AMQP formatter fix explicitly sets NULL value length to zero so callers do not depend on stale/uninitialized length state. Corroborates total initialization of out-parameters/result metadata on every variant. |
| !11365 | Scanned, merged | WiX installer build fix. Build-specific; no new convention extracted. |
| !11364 | Scanned, merged | Display-filter lexer/parser change permits subtraction without a preceding space. Language behavior fix; no broader convention extracted. |
| !11363 | Deep, merged | John Thacker fixes XMPP temporary GHashTable/GList leakage when recursive child creation throws. Strong corroboration for existing exception-safe cleanup/allocator-scope guidance; no duplicate rule added. |

## Frontier

`!11362` (`COPS: Use a wmem_array`) exists in the same corpus snapshot and is merged. It was inspected only as a frontier probe and is **not** part of this run's reviewed set. The corpus therefore has not run out, and no scraper restart is needed for this run.
