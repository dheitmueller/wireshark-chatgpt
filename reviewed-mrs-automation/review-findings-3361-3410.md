# Review findings: !3361–!3410

Model: GPT-5.6 Sol
Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Merged changes are weighted more strongly than closed/superseded work. Direct architecture and correctness guidance from Guy Harris and other maintainers is treated as high-authority evidence.

| MR | Outcome / weight | Finding |
|---|---|---|
| !3410 | Merged / scanned | Gerald Combs CI update adds 32/64-bit PortableApps artifacts to signature verification and SHA-256 package enumeration; packaging-specific, no substantive human review. |
| !3409 | Merged / deep | Guy Harris precisely documents pcapng option cardinality and Wiretap add/set semantics: add creates an instance, set changes an existing single instance, and multi-instance options use add plus set_nth. |
| !3408 | Merged / scanned | Gerald Combs adds distinct 32/64-bit PortableApps package names, templates, CI packaging, and matching developer documentation. |
| !3407 | Merged / deep | Pascal Quantin fixes NGAP-over-HTTP/2 when several NGAP/N2 objects share a packet by matching the HTTP content-id against the correct JSON object/array; Anders Broman regression-tested old traces. |
| !3406 | Closed / down-weighted | John Thacker draft proposes conversation+direction+fragment-ID reassembly keys rather than mutable packet endpoints. Useful history, but unmerged and superseded by later accepted reassembly work. |
| !3405 | Closed / superseded | CAN-specific tables drew Guy Harris approval, while Lars Völker identified duplicated carrier dispatch and proposed a shared helper across SocketCAN/TECMP/IEEE1722/CANETH. Explicitly continued by merged !3668, which is stronger precedent. |
| !3404 | Merged / deep | Guy Harris removes misleading exception-handler source locations from dissector-bug warnings; deferred diagnostics should not pretend the generic logging/catch site is where the bug occurred. |
| !3403 | Merged / scanned | Guy Harris removes a stray semicolon after an inline assertion helper; narrow syntax cleanup. |
| !3402 | Merged / corroborating backport | Guy Harris release-3.2 backport of the LINKTYPE_ERF timestamp-provenance precision fix represented by !3399. |
| !3401 | Merged / corroborating backport | Guy Harris release-3.4 backport of the LINKTYPE_ERF timestamp-provenance precision fix represented by !3399. |
| !3400 | Merged / scanned | João Valverde makes default logger identity/severity behavior explicit; small logging-state cleanup. |
| !3399 | Merged / deep | Guy Harris sets record precision from the ERF record that actually supplied the timestamp, not from the outer pcap/pcapng wrapper. Timestamp provenance governs precision metadata. |
| !3398 | Merged / scanned | João Valverde fixes wslog domain filtering; focused logging correction. |
| !3397 | Merged / corroborating backport | Guy Harris release-3.4 backport of the ERF helper error-propagation work in !3396. |
| !3396 | Merged / deep | Guy Harris threads err/err_info through ERF helper layers, stops on nested failures, frees temporary arrays/lists on unwind, and classifies violated internal preconditions as WTAP_ERR_INTERNAL. |
| !3395 | Merged / corroborating backport | Guy Harris release-3.4 backport of !3394's LINKTYPE_ERF interface-metadata fix. |
| !3394 | Merged / deep | Guy Harris stops creating a fake IDB for LINKTYPE_ERF pcap input when the encapsulated ERF records can supply the authoritative interface information. |
| !3393 | Merged / discussion-focused | João Valverde rejects compile-time stripping of dot11 debug logging; runtime filtering avoids bitrot/rebuilds, while expensive hex-dump allocation is guarded by ws_log_message_is_active(). Author benchmarked ~800k encrypted frames. |
| !3392 | Merged / scanned | João Valverde migrates lingering GLib logging references to Wireshark logging; Guy Harris corrects one characterization but accepts the cleanup. |
| !3391 | Merged / scanned | Broad migration from g_assert() to Wireshark's ws_assert() abstraction across the tree; no substantive review discussion. |
| !3390 | Merged / scanned | Removed packet-editor preference is registered obsolete so old profiles remain parseable; later MRs provide stronger preference-compatibility precedent. |
| !3389 | Merged / corroborating backport | Guy Harris release-3.2 backport setting newly created ERF IDB timestamp precision to nanoseconds. |
| !3388 | Merged / corroborating backport | Guy Harris release-3.4 backport setting newly created ERF IDB timestamp precision to nanoseconds. |
| !3387 | Merged / deep | Guy Harris sets timestamp precision on a newly created ERF Interface Description Block, keeping interface metadata aligned with ERF timestamp semantics. |
| !3386 | Merged / scanned | João Valverde adds domain-aware active-message checks and inverted debug/noisy matches; infrastructure for runtime logging policy. |
| !3385 | Merged / scanned | Gerald Combs removes the generated authors list from wireshark(1); documentation/build cleanup. |
| !3384 | Merged / scanned | Guy Harris updates a pcapng comment after naming cleanup; no new behavior. |
| !3383 | Merged / scanned | Guy Harris simplifies custom-block naming to WTAP_BLOCK_CUSTOM; semantic naming consistency. |
| !3382 | Merged / scanned | Guy Harris renames systemd journal structures/constants consistently to the specification's 'systemd journal export block' terminology across Wiretap, dissectors, extcap, and UI. |
| !3381 | Merged / scanned | Adds the missing wslog header to preference utilities; narrow include dependency fix. |
| !3380 | Merged / scanned | Removes the packet-editor preference; !3390 follows by preserving the old key as obsolete for profile compatibility. |
| !3379 | Merged / scanned | Extends OSPFv3 Authentication Trailer recognition to Database Description packets; protocol-specific correction. |
| !3378 | Merged / scanned | Lowercases Chocolatey package IDs in documentation; documentation-only. |
| !3377 | Merged / scanned | Anders Broman adds several Diameter 3GPP AVPs; routine dictionary update. |
| !3376 | Merged / discussion history | Reverts an accidental 'test' commit that landed with !3367, providing a concrete submission-hygiene example: experimental commits must not remain in the mergeable topic history. |
| !3375 | Merged / scanned | Guy Harris indentation-only pcapng cleanup. |
| !3374 | Merged / corroborating cleanup | Guy Harris removes now-redundant per-handler pcapng block-length rounding after !3372 centralized that normalization. |
| !3373 | Merged / deep | Guy Harris centralizes pcapng option header/length/padding reads, leaving block-specific callbacks with normalized option code/length/content instead of duplicated framing mechanics. |
| !3372 | Merged / deep | Guy Harris centralizes pcapng 4-byte block-length normalization, including deliberate tolerance for malformed legacy files, so individual block handlers no longer repeat container framing logic. |
| !3371 | Merged / scanned | João Valverde expands shared logging with domain/debug/noisy/fatal filters and common documentation; foundational logging infrastructure. |
| !3370 | Merged / discussion-focused | Guy Harris identifies an architectural bug after merge: --capture-comment describes capture-file metadata, not live-capture state, and placing it in global_capture_opts breaks no-pcap builds. Later Guy-authored fixes are stronger accepted precedent. |
| !3369 | Closed / down-weighted | Qt/Lua menu move was closed in favor of broader menu restructuring tracked elsewhere; no accepted implementation precedent. |
| !3368 | Merged / scanned | Small null-pointer fix in frame_data_sequence; no substantive review discussion. |
| !3367 | Merged / discussion-focused | CredSSP ASN.1 work included a sample capture and Anders Broman generator-pattern discussion, but an experimental 'test' commit accidentally landed and had to be reverted by !3376. |
| !3366 | Merged / deep | Pascal Quantin and Jaap Keuter shape the HiPerConTracer/ICMP heuristic: portable 64-bit constants, lean helper structure, release-note placement, append nested protocol column text, squash history, and—critically—preserve parent offset advancement when heuristic dispatch succeeds. |
| !3365 | Merged / scanned | Developer-guide clarification of CRT/UCRT terminology; documentation-only. |
| !3364 | Merged / scanned | Exports a GTPv2 helper for custom dissectors; small API-surface change without substantive review. |
| !3363 | Merged / scanned | ITS curvature 'unavailable' presentation fix; protocol-specific. |
| !3362 | Merged / scanned | Guy Harris pcapng indentation fix. |
| !3361 | Merged / scanned | NGAP adds additional N2SmInfoType handling through ASN.1/config/template plus regenerated dissector output; source-of-truth discipline, no substantive human review. |

## Durable themes

### Capture metadata provenance
Guy Harris's !3387/!3388/!3389 and !3399/!3401/!3402 form a coherent timestamp series: ERF supplies its own timestamp semantics even when carried inside a pcap wrapper, so both per-record precision and generated IDB precision must reflect ERF rather than outer-container defaults. !3394/!3395 extend the same provenance rule to interface metadata by refusing to fabricate an IDB when the encapsulated records can supply the authoritative interface description.

### pcapng common framing and option semantics
Guy-authored !3372 and !3373 normalize block alignment and option framing in common pcapng code before subtype callbacks. !3409 sharpens the typed option contract: option cardinality belongs to the format specification, while add/set/set_nth operations have distinct create-versus-update semantics.

### Error propagation and diagnostic provenance
Guy-authored !3396 makes nested ERF helper failures explicit, carries err/err_info to the owning caller, and unwinds temporary state. !3404 separately removes misleading source-location metadata when an exception is logged later from a generic catch site; diagnostics should identify the meaningful failure provenance rather than incidental reporting machinery.

### Heuristic integration is also parser-control-flow integration
In !3366 Pascal Quantin caught that replacing a direct child dissector call with heuristic dispatch stopped advancing the parent's offset on the successful heuristic path. A heuristic integration must preserve the original caller's consumption/cursor contract as well as recognition semantics. For a nested protocol, protocol-column presentation should append rather than erase the parent protocol's identity.

### Submission history should contain only intended changes
!3367 accidentally carried an experimental commit named "test"; Stig Bjørlykke called out the opaque commit, and !3376 had to revert it after merge. This is concrete evidence for cleaning/squashing topic history before submission and keeping experiments out of mergeable commits.
