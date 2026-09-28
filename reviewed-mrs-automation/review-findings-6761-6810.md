# Wireshark MR review findings: !6761-!6810

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

All 50 MRs in this batch are merged. Merged work is treated as accepted evidence; maintainer-authored and direct maintainer review receives extra weight.

| MR | Depth | Finding |
|---|---|---|
| !6810 | Deep / high-authority | Gerald Combs disables `mmdbresolve` in fuzz and No-options CI builds after it began causing timeouts, keeping those jobs focused on intended coverage. |
| !6809 | Scanned | Release-3.6 RPM packaging normalizes Fedora/SUSE out-of-source build-directory macros and install paths. |
| !6808 | Scanned | Master RPM spec handles SUSE 15.1 build-directory behavior for make/ninja. |
| !6807 | Scanned | Release-3.6 RPM packaging declares guide build dependencies. |
| !6806 | Scanned | Release-3.6 RPM GLib build requirement is synchronized with the project baseline. |
| !6805 | Scanned | Release-3.6 RPM spec removes redundant explicit runtime Requires in favor of generated shared-library dependencies. |
| !6804 | Scanned | Stable build fix adds the missing `string.h` declaration required for `memcpy`. |
| !6803 | Scanned | Release-3.6 RPM packaging stops hard-coding `/usr/local` and honors the RPM prefix. |
| !6802 | Scanned | Gerald Combs extends the html2text helper with parser state for suffixes and row boundaries. |
| !6801 | Scanned | Automatic release-3.6 data/translation update; no new durable convention. |
| !6800 | Scanned | Automatic master data/translation update; no new durable convention. |
| !6799 | Scanned | Automatic release-3.4 registry/data update; no new durable convention. |
| !6798 | Discussion-focused | WSLua uses a narrow volatile-pointer fix for GCC setjmp/longjmp clobber analysis under `-Werror`. |
| !6797 | Deep | John Thacker raises nghttp2 to 1.11.0 and removes the pre-1.11 compatibility wrapper. |
| !6796 | Scanned | 802.11 TWT Setup removes a duplicate Dialog Token decode. |
| !6795 | Deep | RPM build requirements are aligned with the new GLib/CMake baselines and obsolete RHEL 7 workarounds are deleted. |
| !6794 | Scanned | Developer Guide filenames and GNOME HIG link are updated. |
| !6793 | Deep | John Thacker raises GnuTLS to 3.5.8 and removes impossible compatibility branches. |
| !6792 | Deep / very high-authority | Guy Harris adds optional Wiretap section-number metadata with a presence flag, fills it in both sequential and seek-read pcapng paths, stores it 0-based, and displays it 1-based only when valid. |
| !6791 | Scanned | Developer Guide removes obsolete 32-bit Windows references and rewrites dependency setup guidance. |
| !6790 | Scanned | CIP Safety naming, units, value decoding, and field abbreviations are corrected to match the specification. |
| !6789 | Deep / high-authority | Gerald Combs makes unsupported 32-bit Windows a configure-time error instead of allowing the target to continue into the build. |
| !6788 | Scanned | Capture Options owns its initial tab state instead of requiring external callers to reset it. |
| !6787 | Deep | John Thacker raises GLib to 2.50 and removes old fallback logic made unreachable by the new baseline. |
| !6786 | Scanned | Interface activity sorting becomes an explicit per-view model option rather than an unconditional default. |
| !6785 | Scanned | Dead sparkline signal/slot plumbing is removed after the delegate moved to direct model access. |
| !6784 | Deep | John Thacker raises CMake to 3.10, removes obsolete policy/version conditionals, and uses newer comparison behavior guaranteed by the baseline. |
| !6783 | Scanned | Release-3.4 backport of the Sparkle 2 minimum/support cleanup. |
| !6782 | Scanned | Release-3.6 backport of the Sparkle 2 minimum/support cleanup. |
| !6781 | Deep | Master drops Sparkle 1 support and makes Sparkle 2 the minimum, removing the old dual-path implementation. |
| !6780 | Deep | Release-3.4 backport makes conversation proto-data APIs diagnose a NULL conversation as a dissector bug and documents the non-NULL precondition. |
| !6779 | Deep | Release-3.6 backport of the same conversation proto-data NULL-precondition enforcement as !6780. |
| !6778 | Deep / discussion-focused | Couchbase Snapshot Marker parsing changes from one exact 36-byte layout to a required 20-byte prefix plus optional 16-byte and 8-byte suffixes. Alexis La Goutte also requires the contributor to correct commit author metadata and verify the account before merge. |
| !6777 | Scanned | Welcome-page sparklines prioritize active interfaces and add hide/unhide behavior. |
| !6776 | Deep / high-confidence | John Thacker filters non-protocol nodes before protocol-hierarchy recursion and uses `proto_registrar_is_protocol()` rather than display-name heuristics, reducing unnecessary recursion and stack-overflow exposure. |
| !6775 | Deep | After libgcrypt 1.8 becomes mandatory, source paths guarded by now-always-true AEAD/ChaCha capability macros are removed. |
| !6774 | Deep | Tests remove skip logic for libgcrypt versions older than the now-required 1.8 baseline, keeping the test matrix aligned with supported configurations. |
| !6773 | Deep / high-confidence | John Thacker raises libgcrypt to 1.8 and removes extensive pre-1.8 source fallbacks, feature stubs, and compatibility guards; older systems remain covered by the 3.6 LTS branch. |
| !6772 | Scanned | macOS setup script is synchronized with the new Qt 5.9/macOS 10.10 minimum and drops obsolete older-platform setup paths. |
| !6771 | Deep | John Thacker raises Qt to 5.9 and macOS to 10.10, then removes version branches that can no longer execute. |
| !6770 | Discussion-focused | PFM-SD adds the RFC 8364 no-forward bit. The contributor supplies a focused pcap and screenshot; discussion is mainly account/CI logistics. |
| !6769 | Scanned | Removes a second redundant C++11 requirement because the project baseline is already declared centrally. |
| !6768 | Scanned | Sparkle 2 packaging/signing cleanup removes unneeded XPC services for the non-sandboxed application. |
| !6767 | Scanned | macOS setup defaults and commentary are synchronized with Sparkle 2. |
| !6766 | Scanned | Release-3.4 Sparkle 2 application-bundle update. |
| !6765 | Scanned | Release-3.6 Sparkle 2 application-bundle update. |
| !6764 | Scanned | Sparkle 2 bundle signing gains XPC and Brotli-path fixups. |
| !6763 | Deep | CIP Safety mutates running timestamp/rollover state only on the first pass, stores the derived rollover per packet, and reuses it on redissection; rollover stays zero until protocol synchronization establishes state. |
| !6762 | Scanned | Master Sparkle 2 bundle packaging update. |
| !6761 | Discussion-focused | GTP TPDU refactor extracts dispatch helpers without changing behavior; Anders Broman requests clearer helper names that encode TPDU purpose and dispatch target, and the contributor applies the change. |

## Highest-confidence durable evidence

The strongest reusable evidence is !6792 (Guy Harris: optional Wiretap record metadata needs an explicit validity flag and parity between sequential and random-access reads), !6776 (John Thacker: protocol-only traversals should reject non-protocol nodes before recursion using semantic registry predicates), !6763 (first-pass evolving state plus per-packet snapshots for redissection), !6773/!6774/!6775 together with !6771/!6784/!6787/!6793/!6797 (raise dependency floors coherently and delete unreachable compatibility/test branches), !6789 (fail unsupported targets during configuration), and !6779/!6780 (conversation proto-data APIs require a real conversation and treat NULL as a programming error).
