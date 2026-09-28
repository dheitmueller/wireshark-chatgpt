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
