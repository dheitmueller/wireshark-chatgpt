# Wireshark MR review findings: !1760-!1809

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

Merged work is weighted more heavily than closed/superseded work. Direct maintainer guidance, especially from Guy Harris and other long-standing maintainers, is weighted strongly.

## Strong durable findings

### !1787 — keep command-line grammar unambiguous

Merged !1787 adds repeatable `--hexdump <hexoption>` modes. Dario Lombardo asked whether legacy `-x` could instead take an optional argument. Guy Harris identified the ambiguity this creates because TShark also accepts trailing capture-filter syntax: a token such as `host` after `-x` could be either an optional `-x` argument or the first word of a capture filter. The accepted design uses a distinct long option with an explicit argument.

John Thacker also caught two public-interface details: the hexdump must still go to stdout when `-w` writes packets to a file, and documentation examples must not rely on `strptime` formats unavailable on supported platforms.

**Durable rule:** do not retrofit optional arguments onto short options when remaining argv has independent positional/filter meaning. Prefer an explicit long option with a required semantic argument. Treat parser grammar, documented examples, stdout/stderr behavior, and supported-platform formatting as one CLI contract.

Closed !1784 is the superseded precursor and is used only for discussion/history.

### !1774 / !1785 — optional subsystem initialization must be demand-driven

Merged !1774 made TShark register extcap preferences unconditionally so those preferences existed before the preferences file was parsed; !1785 backported it. Peter Wu questioned the startup cost. Gerald Combs measured Windows ctest growing from roughly three minutes to eight or nine minutes. Guy Harris proposed deferring unresolved `extcap.` settings until extcap information is actually needed; Gerald proposed scanning early options and later implemented the demand-driven direction in merged !2035.

**Durable rule:** a functionally correct initialization change is not sufficient when an optional/process-backed subsystem imposes a large startup cost. Preserve preference/configuration semantics with targeted early/deferred registration and initialize expensive optional machinery only when requested behavior needs it. Later merged !2035 remains the preferred implementation precedent.

### !1768 -> !1771 — TCP segment boundaries are not application PDU boundaries

Merged !1768 initially stripped a four-byte E1AP length indication and immediately called the PDU dissector. Pascal Quantin explicitly noted that TCP is a stream and directed use of `tcp_dissect_pdus()`; he then authored merged !1771 with a fixed-header/PDU-length callback and a per-PDU dissector.

**Durable rule:** for deterministic length-delimited TCP protocols, express the minimum header and complete-PDU length and let `tcp_dissect_pdus()` handle segmentation, coalescing, and repeated PDUs rather than treating TCP segments as PDUs.

### !1760 -> !1783 — typed-item checker findings are wire-model questions

Merged VeNCrypt support !1760 introduced fields whose registered types did not match the lengths used by `proto_tree_add_item()`. Martin Mathieson cited the exact `check_typed_item_calls.py` failures. Merged !1783 corrected the model by splitting a 16-byte TightVNC tunnel record into a 32-bit code plus textual vendor/signature fields and by changing a four-byte VeNCrypt authentication type from `FT_UINT8` to `FT_UINT32`.

Jaap Keuter also objected to unrelated history riding in the same MR because it harms selective backporting; the contributor rebased it away.

**Durable rule:** solve typed-item mismatches by verifying and modeling the actual wire structure, not by changing a length merely to appease the checker. Keep independent changes separable enough for backporting.

### !1801 / !1803 / !1775 — documentation must explain use, not merely existence

In the Resolved Addresses, DNS statistics, and DHCP statistics MRs, Peter Wu, Moshe Kaplan, and Anders Broman repeatedly rejected descriptions that merely said a window showed "related data." Review asked for the actual dimensions shown, where the data comes from, and examples of how a reader could interpret/use it. Peter also recommended addressing the reader directly ("you") and using internal cross-references where offline documentation must remain usable.

**Durable rule:** user documentation should explain observable content and practical interpretation, not merely name a feature. Prefer reader-directed wording and internal documentation anchors for project material that must work offline.

## Per-MR accounting

| MR | Outcome | Review result |
|---|---|---|
| !1809 | merged | Gerald Combs replaces manually freed GLib-formatted USB HID temporary strings with packet-scope `wmem_strdup_printf()`; strong allocator-scope corroboration. |
| !1808 | merged | Bluetooth control-procedure collision tracking persists instant/frame state across packets; protocol-specific state-machine work. |
| !1807 | merged | Makes internal ZigBee/ZRTP tables/helpers static and removes unused registrations; ordinary symbol-scope cleanup. |
| !1806 | merged | Corrects AMQP Exchange.UnbindOk method ID 41 -> 51; protocol-specific correction. |
| !1805 | merged | Spelling checker applies eligible-file and generated-file filters to requested/staged files. |
| !1804 | merged | Signal PDU dissector addition; review focused on naming, a config-load bug, and rebasing. |
| !1803 | merged | DNS statistics documentation; strong usability-oriented documentation review from Peter Wu and Moshe Kaplan. |
| !1802 | merged | ANCP statistics documentation; reviewers again ask for concrete statistics/use rather than "window exists" prose; SME limits kept result modest. |
| !1801 | merged | Resolved Addresses docs explain address sources/settings/config files; Peter Wu requests reader-directed wording and offline-safe internal links. |
| !1800 | merged | Python string comparison changed from identity (`is`) to equality (`==`), found by Semgrep. |
| !1799 | merged | Removes list self-appending during iteration in `check_typed_item_calls.py`. |
| !1798 | merged | Replaces mutable default list argument with `None` plus per-call list creation. |
| !1797 | merged | Same list-growth bug in `check_tfs.py`; Martin Mathieson reports a `MemoryError` and discusses possible, scoped Semgrep CI use. |
| !1796 | merged | Same list-growth bug in `check_spelling.py`; an additional filtering issue is deliberately left for a separate MR. |
| !1795 | merged | Coverity catches SOME/IP copy/paste assignment to the wrong member; author confirms. |
| !1794 | merged | Automatic release-3.2 data/translation update; routine generated maintenance. |
| !1793 | merged | Automatic release-3.4 data/translation update; routine generated maintenance. |
| !1792 | merged | Automatic master data/translation update; routine generated maintenance. |
| !1791 | merged | TCP SACK ranges are incorporated into bytes-in-flight analysis. |
| !1790 | closed | Debug-heavy WIP precursor to merged !1791; down-weighted. |
| !1789 | merged | Guy Harris release-3.2 backport of a comment typo fix; no durable rule. |
| !1788 | merged | Guy Harris release-3.4 backport of the same typo fix; no durable rule. |
| !1787 | merged | TShark `--hexdump` feature; high-value Guy Harris and John Thacker CLI/portability review. |
| !1786 | merged | Master comment typo fix; no durable rule. |
| !1785 | merged | release-3.4 backport of unconditional extcap preference registration. |
| !1784 | closed | First `--hexdump` attempt; useful Guy Harris CLI and branch-cleanup discussion, but superseded by merged !1787. |
| !1783 | merged | Fixes VNC typed-item width/model errors exposed by checker; also cleans unrelated branch history. |
| !1782 | merged | Corrects GSM RSL SRR bit mask; narrow field fix. |
| !1781 | closed | Earlier RSL submission accidentally includes unrelated already-merged history; superseded by clean !1782. |
| !1780 | merged | Gerald Combs ensures dot11decrypt fallback utility code is built with older libgcrypt, fixing undefined references. |
| !1779 | merged | Gerald Combs restores useful rpmbuild verbosity in verbose builds after quiet mode hid the real linker failure. |
| !1778 | merged | release-3.4 backport of F5 trailer heuristic fixes. |
| !1777 | merged | release-3.2 FC ELS fix: WWN field length 6 -> 8. |
| !1776 | merged | release-3.4 backport of the same FC ELS width fix. |
| !1775 | merged | DHCP stats docs; Anders Broman asks for concrete meaning instead of "related data" and notes WIP should mean actually incomplete work. |
| !1774 | merged | Unconditional extcap registration; later measured as a major startup regression and superseded architecturally by !2035. |
| !1773 | merged | Master F5 trailer heuristic fix for short legacy trailers and FCS-prefix alignment. |
| !1772 | merged | 802.11 FILS Discovery feature; routine protocol feature and rebase note. |
| !1771 | merged | Pascal Quantin-authored E1AP `tcp_dissect_pdus()` correction; strong transport-framing corroboration. |
| !1770 | merged | Bluetooth HCI Summary documentation; no distinct rule beyond documentation guidance above. |
| !1769 | merged | Bluetooth Devices documentation; no distinct rule beyond documentation guidance above. |
| !1768 | merged | Initial E1AP-over-TCP wrapper; Pascal identifies missing stream framing and follows with !1771. |
| !1767 | merged | Master FC ELS WWN field length fix. |
| !1766 | merged | 802.11 Reduced Neighbor Report update; protocol-specific. |
| !1765 | merged | Guy Harris moves locals into the blocks where used and removes redundant initialization; authoritative but conventional C/static-analysis cleanup. |
| !1764 | merged | PTP adds a hidden derived 32-bit timestamp and uses `proto_tree_add_item_ret_*`; protocol-specific. |
| !1763 | merged | dot11decrypt Windows build compatibility declaration fix; narrow. |
| !1762 | merged | Guy Harris explicitly casts `__LINE__` before formatting because ISO C does not promise a more specific integer type; narrow portability evidence. |
| !1761 | merged | Guy Harris variable-scope/static-analysis cleanup similar to !1765. |
| !1760 | merged | VeNCrypt support; important chiefly as the source of checker findings corrected by !1783. |

## Weighting and VANC

Closed !1790, !1784, and !1781 are retained only for historical/review evidence; merged successors !1791, !1787, and !1782 carry implementation weight. No SMPTE ST 291/VANC packet type was encountered in this batch.
