# Wireshark Architecture Notes

This is a curated cache of architectural understanding relevant to future development. It is intentionally incomplete; add material when it becomes useful for actual work.

## Operating rule

When changing a subsystem, trace the real call path in the current source before designing the patch. Prefer existing registration, dispatch, reassembly, conversation, and dissector-table mechanisms over adding parallel infrastructure.

## Extension points and protocol coupling

- Prefer generic extension points in a generic protocol dissector over embedding knowledge of one downstream protocol. MR !26376 is moving CoAP away from a dedicated Thread/TMF mode toward a generic heuristic payload subdissector list. The MR is still open, so treat its exact implementation as provisional, but use the separation-of-concerns principle when evaluating new designs.
- Avoid pushing consumer-specific protocol identity or dependencies down into generic transport/security dissectors when the consumer can resolve what it needs through an existing API. In MR !26374, Anders Broman requested that RADIUS resolve its relevant protocol ID locally with `proto_get_id_by_short_name()` rather than changing both TLS and DTLS to expose RADIUS-specific plumbing.
- Registration choices carry semantic meaning. A fixed TCP/UDP port table entry says more than “this often runs here”; do not claim an unassigned/dynamic port. Use Decode As or a well-founded heuristic mechanism instead.
- Treat a dissector entry point as an API contract, including the meaning of its `data` argument. If different registration/call paths supply different data contracts, prefer separate entry points that invoke common implementation code with explicit parameters. Guy Harris rejected pointer-identity/type guessing in !26224 and implemented the separate-entry-point design in merged !26229 for IEEE 802.15.4 FCS handling.
- Prefer existing dissector tables when a protocol field names the encapsulated protocol. Merged !26218 dispatches GUE Variant 0 through `ip.proto`, preserving normal protocol registration and nested display rather than embedding a private protocol switch.
- When nested protocol layers have more than one plausible session context, make that distinction explicit at the API boundary. Merged !25853 separates “current TLS session” from “parent/calling TLS session” and passes the resolved `SslDecryptSession *` into lower helpers rather than making generic `packet_info`-based helpers infer which nesting level the caller intended. Prefer typed, already-resolved context when the semantic choice matters.

## Frontend-specific GUI behavior

Shared GUI bases should implement behavior that is genuinely common to Wireshark and Stratoshark without repeatedly asking at runtime which application is executing them. Put application-specific behavior in frontend subclasses and expose initialization hooks late enough for normal virtual dispatch to reach those overrides.

Merged MR !23006, authored and merged by Michael Mann, refactors the common I/O Graph dialog into a frontend-neutral base plus a Stratoshark-specific subclass. The base no longer branches on application identity; an explicit `initialize()` phase allows overridden frontend behavior to participate after construction. Merged !22994 applies the same architectural direction to the Welcome Page by creating a Stratoshark-specific page instead of making the common page query the current application.

**Architecture rule:** when Wireshark and Stratoshark share a substantial widget/controller but differ in behavior, prefer a common base with frontend-specific specialization over scattered runtime application checks. If virtual behavior is needed during setup, separate construction from an explicit post-construction initialization phase rather than depending on virtual dispatch from a base constructor.

**Confidence:** High. Repeated merged master refactors authored/merged by Michael Mann, with the separation goal stated directly in the MR descriptions.

## Stateful analysis and identity

State/reassembly keys must model the protocol's actual identity tuple, not just whichever field is most obvious. MR !26223 fixed MCTP reassembly where a three-bit tag alone collided between request and response; the tag-owner bit is part of the identity and therefore belongs in the key. When adding stateful analysis, enumerate the complete protocol identity before choosing conversation/reassembly keys.

When a wiretap reader derives packet context only during sequential parsing, do not assume `seek_read()` can reconstruct it from the packet's local byte range. Persist the required per-packet context during the first pass, keyed by the packet's stable `data_offset`, and restore it on random access. Merged !25834 fixes 3GPP nettrace second-pass UE-ID/timestamp corruption with exactly this pattern.

For known-length reassembly, bounds for overlap comparison must be based on bytes that actually exist in the allocated reassembly buffer, not merely on fragment geometry. Merged !25837 clips the overlap end to `datalen` before `memcmp()`, because fragments may legally be recorded beyond the known assembled-data boundary even though no backing buffer exists there.

Sequence-based reassembly must also treat the *aggregate* assembled size as hostile arithmetic. Merged !25844, authored by John Thacker, uses checked addition while summing fragment lengths, caps the result to the implementation's supported `INT32_MAX` domain, and clips subsequent copies to the remaining allocated capacity while flagging overlong fragments. A wrapped total is not merely a bad length value; it can cause under-allocation followed by an out-of-bounds copy. Backports !25846 and !25847 preserve the same rule.

## State transitions and update ordering

When changing configuration that invalidates derived dissector/UI state, determine the effects and quiesce the affected consumers *before* exposing the new values. Merged !25841 fixes preference application that previously mutated stashed preferences while simultaneously discovering whether redissection was required; a dissector could therefore observe a new preference value while still holding resources/state from the old value. The corrected sequence first computes the effect flags, freezes the packet list if redissection is required, and only then applies the preference values. The merged release-4.6 backport !25857 corroborates the ordering. Treat multi-step configuration changes as a state transition, not a series of independently safe assignments.

Merged !25862 tightens this ordering further: `rsaKeysFrame->acceptChanges()` itself contributes redissection effects, so it must run while effect discovery is still being accumulated, before the packet list is conditionally frozen and the remaining preferences are applied. When a configuration subsystem has multiple effect-producing sources, enumerate all of them before taking the transition action; a late-discovered effect can invalidate the safety of an otherwise correct two-phase update.

## API mutation contracts and semantic domains

Helpers with separate output and input pointers must be correct for any aliasing their interface permits. Merged !25873 fixes `nstime_delta()` when the destination is the same object as one input: the old implementation wrote through `delta` and then later read `a`, so `delta == a` changed the source data mid-calculation. The corrected implementation computes from the original input members before mutating dependent output state. When designing or modifying such helpers, either preserve correctness under supported output/input aliasing or make a non-aliasing requirement explicit and enforced; do not accidentally depend on distinct storage.

Normalize representation-specific units into semantic units near the parsing boundary, and carry types that match the value domain. Two merged DCT2000 cleanups authored by Guy Harris provide strong evidence: !25876 changes inherently nonnegative size, offset, and timestamp-related variables to unsigned types, while !25877 converts file character counts into record byte counts once and then uses the byte count throughout downstream record sizing/copy logic. Avoid repeatedly carrying ambiguous “length” values whose unit changes by context; name and type values so later arithmetic expresses the protocol/file semantics directly.

Names for shared conversion constants must encode the conversion direction correctly. Merged MR !24946, authored and merged by Guy Harris, removes `WS_MSECS_PER_NSEC` and `WS_USECS_PER_NSEC` because their values were actually nanoseconds-per-millisecond and nanoseconds-per-microsecond; callers are changed to the correctly named `WS_NSECS_PER_MSEC` and `WS_NSECS_PER_USEC`. The surrounding merged nstime/epoch cleanup series (!24943, !24950, !24920, !24926, !24928) also replaces locally repeated conversion and epoch literals with shared authoritative constants. A mathematically correct value under a reversed or ambiguous unit name is still an API defect because it invites the next caller to use the constant in the wrong direction.

## Per-call scratch state and reentrancy

Mutable working state that has no semantic lifetime across calls should normally be per-call state, not static storage. The merged EAX backports !25932 and !25933 move the `eax_s` decryption workspace from a static object onto the `Eax_Decrypt()` stack and pass it explicitly into helpers. Because no state is intended to persist between decryptions, static storage only creates accidental cross-call coupling and a concurrency risk. Prefer local/per-call scratch objects and explicit context parameters; reserve mutable static/global state for intentionally shared lifetime with appropriate synchronization.

## Robust text conversion

When converting externally supplied byte streams to text for display, prefer a shared conversion path that preserves as much valid data as possible when the source contains malformed byte sequences. Merged !25886 replaces `g_convert_with_fallback()` in Follow Stream because an illegal source sequence can make it return `NULL` and discard the entire buffer; Wireshark's exported `get_string_enc_iconv()` instead recovers from `EILSEQ`, substitutes U+FFFD for the malformed subpart, and continues conversion. Treat malformed input as a local decoding defect rather than a reason to lose otherwise valid surrounding text, and centralize that behavior in the common charset API rather than duplicating ad-hoc recovery in UI callers.

## Wiretap error semantics

Reserve `WTAP_ERR_INTERNAL` for conditions that are impossible if Wireshark's own invariants hold, including when the input file is malformed. Ordinary malformed-file validation belongs under a bad-file error such as `WTAP_ERR_BAD_FILE`; user misconfiguration should likewise not be mislabeled as an internal bug. If one path can fail for both an internal bug and bad input/configuration, distinguish the causes if practical; otherwise prefer the non-internal classification until the code can prove the invariant violation. !25816 first raised this distinction, and the merged !25831 discussion independently corroborated it. GUI-facing wiretap errors should describe the file format/module and user-relevant failure rather than exposing an internal helper-function name.

Merged !25865 supplies direct implementation corroboration: TTL validity states that are impossible at those internal call sites were changed from `WTAP_ERR_BAD_FILE` to `WTAP_ERR_INTERNAL`. This sharpens the rule: classify the condition according to where the violated contract resides, not merely because malformed capture data was involved somewhere earlier in the call chain.

Malformed capture-file syntax is an ordinary input error, not a reason for a parser/scanner to terminate the Wireshark process. Merged !25907, authored and merged by John Thacker, changes the Busmaster scanner's unterminated-header path from Flex `YY_FATAL_ERROR()` to setting `WTAP_ERR_BAD_FILE`, supplying `err_info`, and terminating only the local scan. Wiretap parsers should translate attacker-controlled malformed input into the normal error-return path and let callers decide how to present or recover from it. The release-4.6 and release-4.4 backports !25915 and !25916 preserve the same behavior.

Reader capability and writer capability are independent contracts. A wiretap dumper must validate each record against the format and buffer limits it can actually emit even when the corresponding reader accepts larger records. Merged !25917, authored and merged by John Thacker, guards the K12 writer against records larger than its backing frame buffer and returns `WTAP_ERR_UNWRITABLE_REC_DATA` with a useful size error instead of copying past the allocation. Do not infer “writable” from “readable”; reject unsupported output records before materializing them.

Merged !25930 applies the same taxonomy and writer-capability rule to BLF: a truncated Ethernet record is real input data, not an internal invariant failure, so the dumper returns `WTAP_ERR_UNWRITABLE_REC_DATA`; it also rejects VLAN frames too short for the fields it must read and rejects payload lengths that do not fit BLF's 16-bit representation. The release-4.6 backport !25936 preserves the behavior.

## Tree visibility versus filterability

A protocol-tree item need not be visibly rendered to remain useful for display filtering. MR !26393 demonstrates adding eCPRI fields and calling `proto_item_set_hidden()` when O-RAN FH owns the visible dissection. This is useful when a parent/encapsulation protocol has filter semantics that should remain available without duplicating visible presentation.

Conversely, do not hide raw data merely because a structured decoder was selected in principle. MR !26223 corrected an NVMe-MI path where the raw item disappeared even though the selected decoder rendered nothing. Claim/visibility decisions should reflect what was actually decoded.

## Redissection and allocator-scope lifetime

Containers that survive across dissection passes must not retain references to objects whose allocator scope is reset between those passes. A hash table or map can itself remain allocated while every file-scope value it points at has already been freed; redissection then turns those entries into dangling pointers. Merged MR !26096, authored and merged by Guy Harris, fixes ZigBee Touchlink by clearing the commissioning map in the dissector init callback before a new pass because all of its entries were allocated with file scope. Treat the reset of an allocator scope as a lifetime boundary for every pointer stored elsewhere, and explicitly clear/rebuild any longer-lived indexes or containers that refer into that scope.

This matters even when stale pointers appeared harmless on one allocator/platform. !26096 was exposed by a capture that queued redissection after a later Decryption Secrets Block and crashed on macOS when freed address-space regions became inaccessible. Correct lifetime design must not depend on a particular allocator leaving freed memory mapped.

## Shared extcap control plane

Extcap process-control and structured status handling that must work for Wireshark, tshark, and other frontends belongs in the shared extcap/capture layer rather than a Qt-only UI component. Merged MR !26093, authored and merged by John Thacker, moves control-in pipe handling out of Qt so the same framed protocol can carry toolbar control plus structured log/error/warning commands independent of frontend. Merged !26095 factors the sync-pipe reader into reusable capture-layer code, and merged !26097 builds extcap-base logging and control callbacks on top of that shared channel.

Do not repurpose an extcap's ordinary stdout/stderr as a structured control protocol unless the execution contract guarantees that every byte on the stream is framed accordingly. !26093 notes that arbitrary stderr cannot safely be assumed to contain sync-pipe messages; !26097 further notes that stdout has independent meaning when an extcap is run directly in capture mode from the command line. Prefer a dedicated control pipe whose presence explicitly establishes the structured-message contract, preserving ordinary process streams for their existing CLI/data semantics.

## Areas of particular interest

- `epan/` dissector APIs and protocol-tree construction.
- RTP payload dispatch and media-over-IP dissectors.
- MPEG PES payload dispatch.
- Ancillary-data parsing and code reuse between ST 2110-40 / ST 2038 / related SMPTE transports.
- Test and fuzz target integration.

## Provenance

Architectural claims should ultimately cite current upstream files, commits, or review discussions in prose so stale assumptions are easier to detect and revalidate.