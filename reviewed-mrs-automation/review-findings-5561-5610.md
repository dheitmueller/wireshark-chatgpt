# Wireshark MR review findings 5561-5610

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

This run reviewed exactly 50 previously unreviewed MRs, descending from !5610 through !5561. Merged master work is weighted most heavily; stable backports corroborate master behavior; closed or superseded work is lower-weight evidence.

| MR | State | Review finding |
| --- | --- | --- |
| !5610 | Merged, master | Removes 32-bit MSI production while keeping the public release notes synchronized with the CI packaging surface. Gerald Combs's discussion also treats process-bitness memory limits as a poor substitute for an operationally safe capture topology. |
| !5609 | Merged, master | Consolidates absolute-time display values into field_display_e, removes a parallel enum/header, and adds explicit subset validation. Useful type-domain evidence: when one metadata field carries several display classes, represent the common domain directly and assert the valid subset at APIs that accept only one class. |
| !5608 | Merged, master | Narrows an EditorConfig rule from proto.[ch] to proto.c so formatting policy matches the files that actually require it. |
| !5607 | Merged, master | Removes an obsolete GArray compatibility header and uses the current container representation directly; straightforward cleanup after dependency/API evolution. |
| !5606 | Closed draft | João Valverde and John Thacker's discussion corrected a conceptual misunderstanding of ISO-8601 offsets and then exposed the real reversed-sign bug. Do not treat the draft implementation as precedent; merged !5668 is the authoritative correction and was captured in the preceding run. |
| !5605 | Merged, master | Limits a repetitive text-import warning to one user-facing popup while sending repeats to logging, and normalizes the displayed token. Distinguish actionable interactive notification from repeated diagnostic detail. |
| !5604 | Merged, master | Disables the IPv6 control when no generated IP header can use it. Dependent controls should reflect whether their semantic parent option is active. |
| !5603 | Merged, master | Changes repository-lockdown automation from periodic polling to issue/PR-open events; a workflow should be triggered by the event whose state it enforces when such an event is available. |
| !5602 | Merged, master | Refactors inet conversion helpers around C99/POSIX types, adds deterministic error behavior, capability detection, symbol metadata, and focused IPv4/IPv6 tests. |
| !5601 | Merged, master | Fixes custom IPv6 source/destination propagation by copying the selected addresses according to direction rather than treating the all-zero address as an implicit absence sentinel. |
| !5600 | Merged, master | High-value UI review. Guy Harris points out that an "IPv6" checkbox hides the unchecked IPv4 meaning and recommends an explicit IPv4/IPv6 choice; he also surfaces the no-address/default-address ambiguity. John Thacker's accepted follow-up !5632 implements the clearer semantic option. |
| !5599 | Merged, master | Persists newly introduced source and destination address settings. New configuration UI is incomplete until its semantic state survives the same persistence cycle as peer settings. |
| !5598 | Merged, master | Refreshes GitHub Actions tool/action versions and simplifies artifact upload paths. Mostly CI maintenance rather than durable architecture evidence. |
| !5597 | Merged, master | Introduces ws_strptime so feature-test macros and the system-versus-gnulib choice are owned by one portability layer instead of repeated at every caller. |
| !5596 | Merged, master | Corrects shared diagnostic-option documentation ("noisy" rather than "debug") after !5580; documentation for common options must reflect the actual logging contract. |
| !5595 | Merged, master | Removes a duplicate generated configuration definition; low-level build hygiene. |
| !5594 | Merged, master | Fixes Windows capability detection by replacing link-only check_function_exists probes with header-aware check_symbol_exists probes where APIs may be macros/inlines or affected by calling conventions. |
| !5593 | Merged, master | Updates PFCP decoding to 3GPP TS 29.244 V17.3.0. Protocol-version maintenance with no broader review convention beyond keeping source references and fields synchronized. |
| !5592 | Merged, master | Adds custom IPv4 source/destination inputs with validation and import-button gating, extending the GUI surface consistently with the underlying text-import parameters. |
| !5591 | Merged, master | Wraps public C inet helper declarations in extern "C" for C++ consumers. Public C headers used by C++ must preserve C linkage. |
| !5590 | Merged, master | Corrects "selection" terminology to "highlighting" in both Qt UI and documentation, keeping user-facing vocabulary aligned with actual behavior. |
| !5589 | Merged, release-3.4 | Release-note preparation. Useful release-history evidence but lower architectural weight. |
| !5588 | Merged, release-3.6 | Release-note preparation. Useful release-history evidence but lower architectural weight. |
| !5587 | Merged, master | Moves ASCII-identification state into the hexdump-specific import substructure and exposes/persists it in the GUI, keeping mode-specific options grouped with the mode that consumes them. |
| !5586 | Merged, master | Removes unnecessary AsciiDoc blocks from the Wireshark man page; documentation cleanup only. |
| !5585 | Merged, master | Adds text2pcap Export-PDU support and makes the option establish the matching upper-PDU link type and protocol-name metadata. A CLI option that selects a semantic encapsulation must configure the coupled output metadata coherently. |
| !5584 | Merged, release-3.4 | Makes included documentation prefaces self-contained on the stable branch; corroborates keeping reusable documentation fragments structurally complete. |
| !5583 | Merged, release-3.6 | Stable counterpart of the self-contained-preface change; corroborating evidence only. |
| !5582 | Merged, master | Checks packet addresses before use and moves temporary storage to pinfo->pool. Pascal Quantin explicitly notes the project direction toward pinfo pool and suggests validating before allocating so an early exit does no unnecessary work. |
| !5581 | Merged, master | Master version of the self-contained-preface change; strongest of the three branch variants but still primarily documentation structure. |
| !5580 | Merged, master | Centralizes diagnostic CLI option documentation in one include consumed by many tools and aligns help formatting. Common frontend options should have one reusable documentation source. |
| !5579 | Merged, master | Automatic registry/documentation update; low review value beyond generated-data synchronization. |
| !5578 | Merged, release-3.6 | Automatic registry update backport; low architectural weight. |
| !5577 | Merged, release-3.4 | Automatic registry update backport; low architectural weight. |
| !5576 | Merged, master | Preserves HTTP/2/gRPC analysis when a capture starts after dynamic-table setup by storing bounded per-stream fake-header context. Useful partial-capture resilience evidence, though it had little substantive maintainer discussion. |
| !5575 | Merged, release-3.4 | Backports Guy Harris's RFC7468 line-loop termination fix from !5573, strongly corroborating that the issue warranted stable correction. |
| !5574 | Merged, release-3.6 | Second stable backport of !5573; corroborating evidence. |
| !5573 | Merged, master | High-authority Guy Harris fix: tvb_find_line_end can return a zero-length line without advancing after the end of a tvbuff when reassembly is off. Guard line-scanning loops with tvb_offset_exists so end-of-buffer cannot become an infinite stationary loop. |
| !5572 | Merged, master | Makes the text2pcap round-trip test use the explicit %f fractional-seconds format qualifier, keeping tests aligned with the documented parser grammar. |
| !5571 | Merged, master | Moves text-import debug state into the import-info object rather than a standalone global. Later !5642 replaces the private debug scheme with common logging, so that later design is authoritative. |
| !5570 | Merged, master | Gerald Combs corrects !5565 by removing a premature guint32 cast before the G_MAXUINT32 check. Range validation must occur in the wide source domain before narrowing. |
| !5569 | Merged, master | Updates text2pcap usage to describe %f and the revised time parser accurately. |
| !5568 | Merged, master | Defines OFFSET_NONE behavior and reveals that malformed/random input can generate a storm of slightly different popup warnings; !5605 provides the accepted notification-throttling follow-up. |
| !5567 | Merged, master | High-authority Guy Harris change: parse the IP protocol option directly with ws_strtou8 and route all ways of setting it through a shared setter so range checking and coupled state are centralized. |
| !5566 | Merged, master | High-authority Guy Harris change: represent whether -i was supplied with a separate Boolean rather than an out-of-range sentinel embedded in the protocol value. |
| !5565 | Merged, master, later corrected | Attempted to satisfy Clang by casting strtoul to guint32 before storing in an unsigned-long temporary, which defeated the subsequent large-value check. Treat !5570 as the authoritative correction and this as negative regression evidence. |
| !5564 | Merged, master, superseded | Adds an explicit narrowing cast for the IP protocol value to silence Clang. !5566/!5567 subsequently give the state a proper presence flag and guint8 value, so the later typed design is stronger precedent. |
| !5563 | Closed | Replaces a signed long plus -1 sentinel with an unsigned type but preserves sentinel-style state. Superseded by Guy Harris's merged !5566/!5567; do not use as accepted precedent. |
| !5562 | Closed | Gerald Combs proposed the right broad directions for two compile warnings (width-specific ws_strtou8 and wide intermediate/range check), but the branch was closed. Guy Harris explicitly split/fixed the ideas in merged !5567 and noted the parser argument bug he corrected there. |
| !5561 | Merged, master | Adds OFFSET_NONE with explicit one-packet semantics and documents which later direction/timestamp/offset material is ignored. New modes need a clearly bounded behavioral contract, not just a parser flag. |

Strongest durable lessons from this run are promoted into the topical notebook files and summarized in conventions-5561-5610.md.
