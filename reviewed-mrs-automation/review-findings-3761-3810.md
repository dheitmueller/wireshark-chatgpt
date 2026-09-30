# Wireshark MR review findings: !3761-!3810

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
Model: GPT-5.6 Sol
Date: 2026-09-30

Merged work is weighted more heavily than closed or superseded submissions. Maintainer-authored changes and substantive maintainer review are weighted by authority; Guy Harris-authored changes are called out where they establish especially strong precedent.

| MR | Outcome | Depth | Findings |
|---|---|---|---|
| !3810 | merged | Discussion-focused | PROFINET Profidrive expansion. Jaap Keuter required file-local indentation style and removal of trailing whitespace, recommended the submission git hook, and Stig Bjørlykke asked that review/fixup commits be squashed rather than polluting history. Useful submission-hygiene corroboration. |
| !3809 | merged | Scanned | EPUB cover and metadata work. Gerald Combs suggested a reusable Wireshark visual asset; final implementation used dedicated cover artwork. Documentation-specific, no broader convention promoted. |
| !3808 | merged | Scanned | Automated release-3.2 registry/translation refresh. Generated maintenance update; no new durable convention. |
| !3807 | merged | Scanned | Automated release-3.4 registry/translation refresh. Generated maintenance update; no new durable convention. |
| !3806 | merged | Deep | ORAN guards reserved `numBundPrb == 0` before arithmetic that would divide by it and adds expert information. Protocol-invalid numeric values must be diagnosed before they enter arithmetic whose preconditions they violate. |
| !3805 | merged | Scanned | Automated master registry/translation refresh. No additional durable lesson. |
| !3804 | closed | Discussion-focused | Superseded PROFINET attempt. Jaap Keuter identified the submitted source as a merge-conflict result rather than valid C source. Low-weight negative submission evidence; merged !3810 is the implementation authority. |
| !3803 | closed | Discussion-focused | Draft browser SSLKEYLOG Lua launcher. Gerald Combs tested macOS/Windows but ultimately moved it to the Contrib wiki instead of shipping it with Wireshark. Useful product-boundary history, but unmerged. |
| !3802 | merged | Deep | Martin Mathieson splits checker findings into warnings and errors; only definite bugs return failure to CI. Establishes that checker fatality should reflect confidence in semantic invalidity, not mere suspiciousness. |
| !3801 | merged | Scanned | ASTERIX item label correction only. No new cross-cutting convention. |
| !3800 | merged | Deep | Guy Harris makes the Meson GLib build carry the same deployment-target and SDK flags as the autotools path. Alternative supported build backends must preserve target-platform semantics rather than merely compile successfully. |
| !3799 | merged | Deep | Guy Harris generates `libffi.pc` for the macOS-provided libffi so GLib's actual discovery mechanism, pkg-config, sees the platform dependency. Capability integration should satisfy the consumer's authoritative discovery path instead of bypassing it with ad-hoc variables. |
| !3798 | merged | Discussion-focused | Checker-driven fixes for duplicate/copy-paste display-filter abbreviations. Historical evidence only: later, stronger review establishes registered filter names as compatibility surfaces, so this old cleanup is not precedent for cosmetic renaming. |
| !3797 | merged | Deep | Guy Harris resolves the Meson executable installed by the chosen Python and records a `meson-done` marker; uninstall removes Meson only when this setup script installed it. Bootstrap scripts must track resource provenance and must not remove user/system tools they do not own. |
| !3796 | merged | Deep | Guy Harris changes the Python probe from generic `python3` to `/usr/bin/python3` because the question is specifically whether Apple supplies Python. Probe the exact provider whose ownership changes setup behavior; an unrelated PATH executable is not equivalent evidence. |
| !3795 | merged | Deep | Guy Harris adds Meson/Ninja support for newer GLib. Gerald Combs' review exposes the need to preserve SDK/deployment flags; Guy confirms and follows through in !3800. Also documents that GLib's Meson logic treats pkg-config as authoritative for libffi. |
| !3794 | merged | Scanned | Initial Xcode-provided Python handling. Useful precursor, but Guy's !3796 immediately refines the probe to the exact Apple path and is the stronger authority. |
| !3793 | merged | Deep | João Valverde adds a public logging helper that bypasses filtering only after callers perform `ws_log_msg_is_active()`. The bypass API documents its precondition and updates the exported symbol list; performance shortcuts must make caller obligations explicit. |
| !3792 | merged | Scanned | Release-note entry for Google Season of Docs 2020. No durable engineering convention. |
| !3791 | merged | Deep | Martin Mathieson and Pascal Quantin distinguish real mask-width errors from stylistic/suspicious mask warnings with valid exceptions. This directly motivates the warning/error split later merged in !3802. |
| !3790 | merged | Deep | QUIC connection hashing fixes the byte range so the hash covers the intended CID identity. Review discusses matching the equality domain. Hash functions for keyed containers must use the same semantic identity domain and correct byte boundaries as equality. |
| !3789 | merged | Deep | João Valverde moves generic byte-to-string formatting from EPAN to wsutil, moves exported symbols, renames APIs to match semantics, and adds unit tests. A layering move of public utility code includes ABI manifests, callers, naming, and tests. |
| !3788 | merged | Scanned | wslog macro de-duplication. Small implementation cleanup with no additional durable rule. |
| !3787 | merged | Scanned | ENIP updates from the current specification. Protocol-specific update without reusable review guidance. |
| !3786 | merged | Discussion-focused | DISv7 expansion. Alexis La Goutte requested TFS reuse and correct provenance; Anders Broman required a rebase; Stig Bjørlykke questioned unsquashed review commits. Mostly corroborates existing submission/provenance conventions. |
| !3785 | merged | Discussion-focused | Adds `ws_log_buffer()`. Guy Harris explicitly probes whether the helper is broadly reusable and whether “buffer” clearly means octets rendered in hex. Public helper additions should justify reuse and precise naming, but this is modest evidence. |
| !3784 | merged | Scanned | DoIP address-name fields. Protocol-specific enhancement; no new durable convention. |
| !3783 | merged | Deep | Broad migration from ambient `wmem_packet_scope()` to `pinfo->pool` across dissectors. Strong corroboration that packet allocation lifetime should be carried by explicit packet context. |
| !3782 | merged | Scanned | Adds EPUB documentation build targets, CI dependency installation, artifacts, and publication. Sound feature integration but no new convention beyond existing build/CI rules. |
| !3781 | merged | Discussion-focused | Debian symbol-list correction confirms `wmem_epan_scope`, `wmem_file_scope`, and `wmem_packet_scope` remained libwireshark symbols. ABI manifests must reflect the actual library ownership after refactors. |
| !3780 | merged | Deep | PCEP moves from an Internet-Draft encoding to RFC 8664 while deliberately retaining the deprecated TLV decoder for real older implementations and supplies a Cisco capture exercising both. Protocol evolution should preserve deployed legacy decode paths when practical and test both generations. |
| !3779 | merged | Scanned | ORAN section extension 11; Jaap Keuter corrects PRB terminology. Protocol-specific terminology correction. |
| !3778 | merged | Scanned | release-3.2 backport of RakNet source-range highlighting fix. Backport corroboration only. |
| !3777 | merged | Scanned | release-3.4 backport of RakNet source-range highlighting fix. Backport corroboration only. |
| !3776 | merged | Scanned | Mailmap identity update requested by contributor. No cross-cutting code convention. |
| !3775 | merged | Scanned | Turkish Qt translation integration. Normal localization/resource update; no durable review rule beyond resolving merge conflicts. |
| !3774 | merged | Discussion-focused | Clang Analyzer dead-store fixes. Jaap Keuter notes an intentionally ignored lookup result should not be assigned back to a typed variable. Narrow static-analysis cleanup, not promoted separately. |
| !3773 | merged | Discussion-focused | Master-origin RakNet byte-highlighting offset correction; reviewer immediately requests backports, which are !3777/!3778. Source ranges shown in the tree must match the bytes actually decoded. |
| !3772 | merged | Scanned | NFAPI spelling corrections. No new durable convention. |
| !3771 | merged | Deep | ITS custom value formatting is implemented in ASN.1 conformance/template input and regenerated output. Corroborates generated-code source-of-truth discipline. |
| !3770 | merged | Discussion-focused | RTP dump header converts nanoseconds to microseconds with `/1000`, correcting a unit conversion. Numeric units at serialization boundaries should be explicit and checked dimensionally. |
| !3769 | merged | Scanned | Guy Harris updates Ninja to the first release shipping a fat x86-64/ARM64 macOS binary. Platform-support maintenance, but no novel rule beyond matching supported architectures. |
| !3768 | closed | Discussion-focused | Combined iLBC API-compatibility and clang-check exception change failed CI; author chose to split it. Lower-weight history; later Guy-authored !4273 is the stronger checker/target-matrix precedent. |
| !3767 | merged | Deep | Evan Huus rejects replacing packet-temporary allocations with NULL-scope allocation plus manual free: dissection can abort nonlocally, so `pinfo->pool` guarantees cleanup even when lexical cleanup is skipped. The contributor revises and Evan approves. Strong allocator-lifetime precedent. |
| !3766 | merged | Discussion-focused | Alexis La Goutte rejects a local macro that combines tree insertion and offset advance and points the contributor to `ptvcursor`; the contributor converts the dissector. Prefer project abstractions for recurring parsing state over one-off macros that duplicate them. |
| !3765 | merged | Deep | Evan Huus converts most ASN.1 dissectors to `pinfo->pool` by changing authoritative ASN.1 templates/configuration and regenerating the derived dissectors. Explicit allocator migrations must reach generator inputs, not just checked-in C. |
| !3764 | merged | Deep | João Valverde resynchronizes ASN.1 generated dissector sources; the diff demonstrates generated C being brought back in line with authoritative inputs. Paired with !3765, this is early concrete evidence that manual generated-output drift will not survive regeneration. |
| !3763 | merged | Discussion-focused | RTPS WAN-locator flags were read from the wrong bit coordinate; fix reads the intended byte before bitmask display. Narrow decoding-coordinate correction. |
| !3762 | closed | Scanned | Earlier PROFINET Profidrive attempt, later superseded by merged !3810. No independent precedent. |
| !3761 | merged | Deep | Guy Harris standardizes dumpcap diagnostics on “capture device,” not “interface,” because Wireshark can capture from abstractions such as “any”, USB, or D-Bus. User-visible terminology should name the full abstraction accepted by the API, not a narrower common case. |
