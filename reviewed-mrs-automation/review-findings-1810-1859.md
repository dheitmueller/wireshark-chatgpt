# Review findings: !1810-!1859

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

Merged MRs are weighted most heavily. Closed !1828/!1819 and still-open !1823 are retained mainly for authoritative review/history and are not treated as accepted implementation precedent.

| MR | Status | Review result |
|---|---|---|
| !1859 | Merged | Release/version bump; no reusable review guidance. |
| !1858 | Merged | 3.2.11 build metadata; no durable convention extracted. |
| !1857 | Merged | 3.4.3 build metadata; no durable convention extracted. |
| !1856 | Merged | RTP exported helpers removed; Pascal Quantin required matching removal from Debian symbol manifest. Durable ABI bookkeeping; removal policy itself treated cautiously. |
| !1855 | Merged | RTP player doubles Qt audio buffer after probing runtime buffer size; narrow UI/audio workaround, no maintainer discussion. |
| !1854 | Merged | Follow DCCP Stream. Pascal Quantin: reset stream counter per capture, queue follow tap before child dispatch, use tests for checked-in capture, separate unrelated Exported PDU work, update docs/generated AUTHORS source, squash/rebase cleanly. |
| !1853 | Merged | NetPerfMeter heuristics generalized across TCP/UDP/DCCP; reviewer steered toward transport-independent heuristic and MR includes capture/tests. |
| !1852 | Merged | Large static-symbol cleanup; internal-only tables/functions made static. |
| !1851 | Merged | 3.2.11 release-note preparation. |
| !1850 | Merged | 3.4.3 release-note preparation. |
| !1849 | Merged | Stable backport of USB HID range-expansion allocation ceiling from !1810. |
| !1848 | Merged | USB HID Usage Minimum/Maximum are inclusive; reject min>max and grow by max-min+1. |
| !1847 | Merged | ZVT data-point cleanup backport; no substantive review. |
| !1846 | Merged | ZVT data-point cleanup on master; no substantive review. |
| !1845 | Merged | Guy Harris stable backport: don't print nanoseconds when seconds conversion is unrepresentable. |
| !1844 | Merged | Same timestamp-formatting backport to release-3.4. |
| !1843 | Merged | Guy Harris master fix: fractional nanoseconds must not be emitted if base seconds cannot be formatted. |
| !1842 | Merged | Guy Harris stable backport replacing Windows gmtime_s path and checking conversion result. |
| !1841 | Merged | Same gmtime portability backport to release-3.4. |
| !1840 | Merged | Guy Harris stable backport: gmtime_s/gmtime_r are fallible for negative/out-of-range time. |
| !1839 | Merged | Same fallible-time-conversion backport to release-3.4. |
| !1838 | Merged | Guy Harris master follow-up: avoid Windows gmtime_s invalid-parameter behavior; use checked non-terminating conversion. |
| !1837 | Merged | Guy Harris master fix: do not assume gmtime_s/gmtime_r succeeds. |
| !1836 | Merged | D-Bus fuzz hardening. Guy Harris: ordinary field-overrun/truncation should use standard TVB/proto-tree exception path; richer generic diagnostics belong in shared exception reporting. |
| !1835 | Merged | Stable backport of ZVT migration to tcp_dissect_pdus(). |
| !1834 | Merged | Stable backport of ZVT migration to tcp_dissect_pdus(). |
| !1833 | Merged | Guy Harris adds missing encap table entry for WTAP_ENCAP_ETW; merged successor to closed !1828. |
| !1832 | Merged | Guy Harris renames WTAP_ENCAP_ETL to semantically correct WTAP_ENCAP_ETW across wiretap/dissectors/extcap. |
| !1831 | Merged | ZVT replaces hand-rolled TCP desegmentation loop with tcp_dissect_pdus() and a protocol length callback. |
| !1830 | Merged | UDP keeps wire checksum value 0 visible while separating ignored/not-present from illegal semantic status via generated status + expert info. |
| !1829 | Merged | Static-symbol checker cleanup. Build exposed that entries in Debian symbols manifest represent exported ABI and cannot simply be made static. |
| !1828 | Closed | Attempted ETW encapsulation table entry. Graham Bloice requested component-prefixed commit message; Guy Harris renamed encapsulation and fixed it in merged !1833. Superseded implementation down-weighted. |
| !1827 | Merged | Guy Harris adds missing connection_info NULL guard in BTLE control-procedure path. |
| !1826 | Merged | Guy Harris 3.2 backport of pcapng sdjournal length/NUL fix. |
| !1825 | Merged | Guy Harris 3.4 backport of pcapng sdjournal length/NUL fix. |
| !1824 | Merged | Adds preference-selectable TCP bytes-in-flight method for incomplete captures; no substantive human review in corpus. |
| !1823 | Open | Waveform-viewer prototype. Guy Harris: standardized pcapng option needs spec acceptance; custom option must process IANA PEN; Windows build conditional should reflect MSVC/Windows reality. Open implementation down-weighted. |
| !1822 | Merged | Gerald Combs pcapng sdjournal fix: reserve payload+NUL capacity without inflating logical length; correct trailing-NUL indexing. Explicitly marked for stable backport. |
| !1821 | Merged | Statistics table init becomes idempotent by finding/reusing existing table; early evidence for create-once/reset/reuse lifecycle later reinforced by broader stats series. |
| !1820 | Merged | Bluetooth event codec labeling correction; no substantive review. |
| !1819 | Closed | Trivial ANSI-A typo fix closed/unmerged; no durable guidance. |
| !1818 | Merged | UDP preference permits ignoring zero checksum over IPv6 while keeping strict behavior as default. |
| !1817 | Merged | Removes unused XMPP globals and makes remaining internal helpers static. |
| !1816 | Merged | NR-RRC security algorithm configuration uses MAC-NR UE ID; authoritative ASN.1 conformance source and generated C changed together. |
| !1815 | Merged | BTLE crash fix: connection context is optional for some captures, so every use path must guard connection_info. |
| !1814 | Merged | ZVT TID bitmap-name backport. |
| !1813 | Merged | ZVT TID bitmap-name backport. |
| !1812 | Merged | USB HID memory leak fix replaces mismatched GLib allocation/free with packet-scope wmem formatted string. |
| !1811 | Merged | ZVT corrects TID meaning from Transaction ID to Terminal ID. |
| !1810 | Merged | Gerald Combs caps USB HID usage-array growth before wmem_array_grow; hardening complements earlier arithmetic-underflow fix. |

## Strongest durable lessons promoted to topical notebook files

- !1854: capture-scoped Follow Stream identity, tap placement before child dispatch, and capture fixtures must be exercised by tests.
- !1810/!1848/!1849: validate inclusive range expansion and enforce a resource ceiling before container growth.
- !1836: normal TVB bounds exceptions are the correct generic mechanism for fields extending beyond captured data.
- !1830/!1818: preserve the actual checksum wire value and model ignored/absent vs illegal as separate semantic states.
- !1823 (open, Guy Harris review): standardized pcapng identifiers require the specification process; custom options require PEN handling.
- !1822/!1825/!1826: reserve terminator capacity separately from logical payload length and index the final payload byte at length-1.
- !1837-!1845: platform time conversion is fallible; do not print fractional seconds without a valid base timestamp.
- !1831/!1834/!1835: prefer tcp_dissect_pdus() to hand-written TCP desegmentation loops when framing is expressible through a length callback.
- !1856: if an exported symbol is intentionally removed, synchronize the packaging symbol manifest, while retaining the separate caution that in-tree non-use alone does not prove an API is safe to remove.

## VANC tracking

No SMPTE ST 291/VANC packet type was encountered in this batch.
