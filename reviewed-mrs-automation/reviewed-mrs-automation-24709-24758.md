# Automated Wireshark MR review: !24709-!24758

## Corpus provenance

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit used for this entire review batch: **`9e52bc78659a888d4eb624984ee1a886a40d959f`**
- Notebook base before this run: **`9785adf07f986d210d82c5de4c0172b42be4ade7`**
- Exactly fifty previously unreviewed MRs were reviewed: **!24709 through !24758 inclusive**.
- Selection was made only after consulting `reviewed-mrs.md`, the aggregate `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run ledgers. Numeric range summaries were not treated as sufficient evidence by themselves.
- The previously reviewed historical batch **!17571-!17620** remains preserved and counted in the reviewed set.

## Exact reviewed set

| MR | Status | Review notes |
|---|---|---|
| !24758 | Deep | Merged John Thacker RTPS hardening. Gives every independently framed submessage a bounded subset TVB and relative coordinate space, reducing overflow risk and preventing malformed parsing from leaking into the next submessage. Promoted to `parser-boundary-conventions.md`. |
| !24757 | Scanned | Merged 802.11 HE 6 GHz regulatory-value labeling improvement. Straightforward presentation mapping; no new durable convention. |
| !24756 | Scanned | Merged John Thacker frame-option extensibility work: PEN-based custom-string dispatch plus generic display for unknown custom string/binary options. Good graceful-unknown behavior, but no new rule beyond existing dispatch guidance. |
| !24755 | Scanned | Merged recursive SMI/MIB-path discovery for libsmi. Useful dependency integration fix; no broad new convention extracted. |
| !24754 | Deep | Merged John Thacker pcap/pcapng length hardening. Uses checked subtraction, treats pseudo-header length exceeding captured length as an internal invariant failure, and safely represents malformed reported length for later frame diagnostics. Reinforces checked arithmetic and wiretap error taxonomy already recorded. |
| !24753 | Scanned | Merged AI-assisted Doxygen header cleanup. Documentation-only; no engineering convention promoted. |
| !24752 | Scanned | Merged RPM setup portability fix makes optional PNG compressors non-fatal when unavailable on a distro. Narrow setup-script portability improvement. |
| !24751 | Scanned | Merged Lua Debugger UI refinement. Presentation-only changes; no durable convention. |
| !24750 | Scanned | Merged stable-branch Fedora build backport removing obsolete `cmake3` assumption after Fedora 44. Corroborates branch/platform CI maintenance. |
| !24749 | Scanned | Merged AI-assisted Doxygen header cleanup. No durable lesson beyond documentation maintenance. |
| !24748 | Scanned | Merged ETL extcap compatibility guard avoids redefining `G_NSECS_PER_SEC` when newer GLib provides it. Straightforward dependency-version compatibility. |
| !24747 | Scanned | Merged RPM setup adds `libnghttp3-devel` opportunistically where available for HTTP/3 support. Packaging-specific optional dependency handling. |
| !24746 | Scanned | Merged stable-branch Fedora/CMake backport. Same accepted fix represented by !24743; no additional lesson. |
| !24745 | Scanned | Merged stable backport of !24740's `INT32_MIN` arithmetic fix. Strong corroboration, but master fix carries the primary lesson. |
| !24744 | Scanned | Merged second stable backport of !24740's signed-minimum fix. No additional implementation lesson. |
| !24743 | Scanned | Merged Fedora build fix drops historical `cmake3` command naming now that supported Fedora releases provide modern CMake as `cmake`. CI maintenance, no new general rule. |
| !24742 | Scanned | Merged stable backport of sidebar recent-preference persistence fix. Defaults must precede loading the common recent file rather than overwrite loaded values afterward. |
| !24741 | Scanned | Merged AI-assisted Doxygen header cleanup. Documentation-only. |
| !24740 | Deep | Merged John Thacker OSS-Fuzz fix for `signed_time_msecs_to_str()`: avoids negating `INT32_MIN` and instead relies on defined C99/C++11 division/remainder semantics. Promoted to `arithmetic-safety-conventions.md`. |
| !24739 | Scanned | Merged master sidebar recent-preference persistence fix. Useful initialization-order example but narrower than existing state-ordering guidance. |
| !24738 | Scanned | Merged documentation of Ctrl+= zoom shortcut. No engineering convention. |
| !24737 | Deep | Merged John Thacker OCP1 OSS-Fuzz hardening. Parses each command from a bounded subset TVB and uses `proto_tree_add_item_ret_*` helpers instead of duplicate value fetches. Strong independent support for bounded-subrecord parsing; promoted with !24758. |
| !24736 | Scanned | Merged backport centralizing application-level preference side effects before emitting change signals. Reinforces existing two-phase/state-ordering guidance. |
| !24735 | Scanned | Merged AI-assisted DIS audio playback/control improvements. Feature/UI work without reusable reviewer correction. |
| !24734 | Scanned | Merged Welcome Page preference-dialog backport. Stable-branch feature plumbing; no new convention. |
| !24733 | Scanned | Merged lossless documentation PNG compression. Resource-only maintenance. |
| !24732 | Scanned | Merged automatic data/translation update. No durable lesson. |
| !24731 | Scanned | Merged automatic data/translation update on another maintained branch. No durable lesson. |
| !24730 | Scanned | Merged automatic data/translation update on another maintained branch. No durable lesson. |
| !24729 | Scanned | Merged stable backport of Welcome banner PNG compression. Resource-only. |
| !24728 | Deep | Merged reassembly correctness fix compares against bytes actually added/materialized rather than the previous fragment's nominal length, avoiding reads of uninitialized reassembly storage. Strong corroboration of the existing reassembly-buffer invariant. |
| !24727 | Scanned | Merged AI-assisted Lua debugger fix separates step-pause and breakpoint-pause state. Useful state-model cleanup but feature-specific. |
| !24726 | Scanned | Merged AI-assisted DIS jitter correction uses transit-time variation and media-derived timing fallback when transmitter timestamps are unreliable. Domain-specific analysis fix. |
| !24725 | Discussion-focused | Merged AI-assisted Lua Debugger modularization splits a monolithic dialog into logical units and leaves a deliberately small public interface. Useful modularity exemplar, but no sufficiently distinct new Wireshark-wide rule. |
| !24724 | Scanned | Merged HiPerConTracer timestamp presentation adds Unix-seconds, Unix-nanoseconds, and raw forms for comparison/filtering. Presentation-oriented. |
| !24723 | Scanned | Merged Windows dependency updates for GLib/libxml2/zlib. Dependency maintenance only. |
| !24722 | Deep | Merged libwireshark/libui dependency cleanup spanning multiple headers. Review also pushed back on introducing an unrelated global `#pragma once` style change in this MR. Strong evidence for preserving library layering and MR scope; existing architecture/submission guidance already covers the general principles, so no duplicate rule added. |
| !24721 | Scanned | Merged stable-branch change raises the legacy Qt 5 build to C++14, narrowing the language-standard gap with Qt 6/Stratoshark while remaining within supported compiler capability. Build-policy maintenance. |
| !24720 | Scanned | Merged stable backport of MACsec size-check fix. No additional lesson beyond master !24716. |
| !24719 | Scanned | Merged stable dependency update for GnuTLS/libgcrypt with a resolved backport conflict. No broader lesson. |
| !24718 | Scanned | Merged CMake copy/paste correction. Low-information maintenance. |
| !24717 | Scanned | Merged second stable GnuTLS/libgcrypt backport. No additional lesson. |
| !24716 | Scanned | Merged master MACsec size-check correction. Defensive-length fix consistent with existing parser-boundary guidance. |
| !24715 | Scanned | Merged CITP live-view content-code correction accompanied by a focused capture. Reinforces established sample-capture validation practice. |
| !24714 | Scanned | Merged master GnuTLS/libgcrypt dependency update. Dependency maintenance only. |
| !24713 | Deep | Merged PROFIsafe corrections fix CRC-dependent trailer sizing, preserve a 32-bit field width, and use raw wire bytes for parser flags rather than coupling control flow to `hf_` bitfield definitions. Strong corroboration for separating decoding semantics from display registration/layout. |
| !24712 | Scanned | Merged PortableApps backport of the Windows ICU exclusion from !24709. Independently confirms the packaging compatibility fix. |
| !24711 | Scanned | Merged master Welcome banner PNG compression. Resource-only. |
| !24710 | Scanned | Merged stable packaging fix installs the sharkd HTML page in the additional location required by Windows packages. Narrow packaging correction. |
| !24709 | Deep | Merged John Thacker Windows packaging fix excludes build-host `icuuc.dll`; application-local loader precedence otherwise caused a newer build-host wrapper to break supported older Windows/Server targets. Promoted to `portability-conventions.md`. |

## Durable notebook changes from this batch

1. **Bound independently framed nested parsers with subset TVBs.** !24758 and !24737 make the parent-provided extent structural by passing a bounded TVB to the child, preventing cross-record leakage and reducing offset-overflow opportunities.
2. **Never negate the minimum value of a signed integer type.** !24740 (plus !24744/!24745) demonstrates a concrete C99/C++11-safe restructuring for the `INT32_MIN` case found by OSS-Fuzz.
3. **Do not package build-host system DLLs as application-private dependencies.** !24709/!24712 show how Windows loader precedence can turn deployment-tool copying into an unintended build-host OS ABI dependency.

Other strong cases were retained as corroboration rather than duplicated: !24754 for checked subtraction and wiretap internal-vs-malformed error semantics; !24728 for comparing only materialized reassembly bytes; !24713 for decoupling parser semantics from `hf_` display definitions; and !24722 for library layering and scope discipline.