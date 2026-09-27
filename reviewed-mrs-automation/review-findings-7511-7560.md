# Review findings: !7511-!7560

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

| MR | Outcome | Weight | Review finding |
|---|---|---|---|
| !7560 | merged | routine | Release build preparation; no new cross-cutting convention. |
| !7559 | merged | routine | Release-note editorial cleanup. |
| !7558 | merged | high | Tomasz Moń moves tshark capture synchronization into the GLib main loop. Event callbacks run on the main thread, so a callback mutex is unnecessary; Windows child shutdown uses a bounded process wait instead of hand polling. |
| !7557 | merged | medium | Display-filter arithmetic semantic checking validates the first operand's operation capability before using its type to validate the second operand, preventing FT_NONE from reaching invalid arithmetic paths. Adds a dedicated FT_NONE pseudofield for regression coverage. |
| !7556 | merged | routine | Removes obsolete EPEL 8 Asciidoctor special casing once the package is available. |
| !7555 | merged | routine | Adds Qt Creator autosave files to gitignore. |
| !7554 | merged | routine | Windows dependency bundle update with pinned archive hash. |
| !7553 | merged/backport | low | Setup-script diagnostic wording and help handling; corroborative backport evidence. |
| !7552 | merged/backport | low | Adds openSUSE build requirements; packaging-specific. |
| !7551 | merged/backport | high | John Thacker changes TLS reassembly to use TCP's reassembly-key functions so first-frame identity is supplemented by the multisegment PDU starting sequence rather than ad hoc bit-mixing. |
| !7550 | merged | medium | Reuses the common E212 MCC/MNC decoder and removes duplicate local parsing code. |
| !7549 | merged/backport | medium | John Thacker fixes setup scripts that dereferenced $1 when no arguments were supplied. Parse arguments without assuming argv[1], make help a normal success path, and perform root checks after help processing. |
| !7548 | merged/backport | high | John Thacker defines TCP reassembly identity using addresses/ports plus first frame and starting sequence. First frame alone can collide when a frame contains multiple encapsulated TCP PDUs; sequence alone can repeat in long/reused connections. |
| !7547 | merged | routine | Corrects copied field label/filter metadata. |
| !7546 | merged | routine | Stable-branch version rollover. |
| !7545 | merged | routine | Stable-branch version rollover. |
| !7544 | merged | routine | Windows dependency bundle updates with hashes. |
| !7543 | closed | low/negative | Superseded TCP reassembly proposal. John Thacker explains why sender-side retransmission classification cannot simply be bypassed: segments outside active reassembly must not be redissected, especially for stateful protocols. Later merged work is stronger authority. |
| !7542 | merged | routine | Ensures the GPL license HTML is installed and removed consistently on Windows. |
| !7541 | merged | routine | 3.4 release build preparation. |
| !7540 | merged | routine | 3.6 release build preparation. |
| !7539 | merged | routine | Uses QTextBrowser so license-page external links can be opened. |
| !7538 | merged | routine | Adds Lua acknowledgement text. |
| !7537 | closed | low | Experimental ftype integer conversions; author closed it as insufficiently valuable. Do not treat implementation as policy. |
| !7536 | merged | medium | Moves acknowledgements to Markdown and packages it across platforms; no stronger code convention. |
| !7535 | merged | routine | Adds the Qt Concurrent development dependency for SUSE packaging. |
| !7534 | merged | medium | Migrates capture regex search from obsolete GRegex to the project PCRE2 wrapper and exports the needed match-position API, keeping regex backend details behind wsutil. |
| !7533 | merged/backport | medium | John Thacker documents TCP desegmentation limitations including rollover, sequence reuse, multiple PDUs per frame, and retransmission ambiguity. Useful rationale for the composite reassembly identity work. |
| !7532 | merged/backport | medium | Corrects gtpv2.smenb from a 24-bit/0x800000 registration to an 8-bit/0x80 masked FT_BOOLEAN. Corroborates that literal Boolean width is appropriate when a real mask/container requires it. |
| !7531 | merged/backport | medium | Same gtpv2 masked-Boolean correction on release-3.6. |
| !7530 | merged | high | John Thacker makes QUIC Follow Stream use stored per-packet datagram/connection state instead of reconstructing identity from addresses/ports, which fails under connection migration and 5-tuple reuse. |
| !7529 | merged | routine | Windows vcpkg bundle refresh; Gerald Combs clarifies the supported bundle selection mechanism is tools/win-setup.ps1. |
| !7528 | merged | medium | Completes RFC 6052 NAT64 prefix handling with UAT validation and explicit prefix/u/suffix fields; review discussion only asks for squash. |
| !7527 | merged | routine | Release-note preparation. |
| !7526 | merged | medium | Master version of the gtpv2 masked-Boolean correction; strongest of the !7526/!7531/!7532 trio. |
| !7525 | merged | medium | Adds MySQL binlog heartbeat event dissection with a focused pcap and before/after screenshots; Alexis La Goutte requests field alignment/consistency before merge. |
| !7524 | merged | routine | Splits PDCP sequence-analysis expert fields by uplink/downlink so diagnostics preserve direction. |
| !7523 | merged | routine | Updates display-filter documentation to match current fvalue_t representation. |
| !7522 | merged | high | Gerald Combs replaces CMake's internal RULE_LAUNCH_COMPILE/LINK mechanism with the documented CMAKE_C/CXX_COMPILER_LAUNCHER and linker-launcher variables for ccache. Prefer supported build-system interfaces over internal properties. |
| !7521 | merged | medium | Adds display-filter double/scientific-notation regression tests. |
| !7520 | merged | routine | Debian packaging CI disables redundant package-test execution to control log/runtime cost. |
| !7519 | merged | routine | Adds TECMP CounterEvent and TimeSyncEvent dissection. |
| !7518 | merged | medium | Improves c-ares CMake discovery with explicit include/lib hints and path suffixes. |
| !7517 | merged | routine | Removes obsolete macOS Sparkle cleanup shim. |
| !7516 | closed | low | Draft float-comparison behavior change was never accepted; do not promote its string-rounding approach. |
| !7515 | merged | high | John Thacker fixes find_or_create_conversation so creation uses the same endpoint/element identity that lookup used. A find-or-create API must not create under a different key than it just searched. |
| !7514 | merged | medium | Normalizes RPM behavior across distributions when git-style versions contain double dashes; avoids distro-specific assumptions. |
| !7513 | merged | routine | Debian packaging stops replacing Wireshark's About-dialog license text. |
| !7512 | merged | routine | Rocky Linux 9 CI uses the actual ninja command name. |
| !7511 | merged | medium | Corpus metadata is inconsistent. The recorded commit/diff removes obsolete Perl from AppVeyor PATH; the title/description instead duplicate the wslua_utility documentation MR. Reviewed according to recorded commit/diff, with anomaly noted in the ledger. |

## Highest-confidence durable lessons

The strongest accepted evidence is !7548/!7551 for reassembly identity, !7530 and !7515 for conversation/follow-state identity, !7558 for event-loop/process handling, and !7522 for using supported CMake launcher interfaces. !7543 is useful negative/superseded discussion evidence only.
