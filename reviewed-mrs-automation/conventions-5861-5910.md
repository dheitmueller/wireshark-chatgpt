# Durable conventions from !5861–!5910

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Highest-confidence additions

- **Malformed packet lengths are not programmer assertions.** Merged !5900, authored by Jaap Keuter, replaces `DISSECTOR_ASSERT()` checks that depended on IPDC packet lengths with ordinary validation. Assertion machinery is for internal invariants; packet data is allowed to be malformed.

- **Guarantee progress even when a corrupt length is zero or shorter than its fixed header.** Fuzz-driven !5885 prevents BLF hangs by rejecting an undersized base header and advancing by at least the fixed structural minimum or declared header length. Merged !5862 and !5863 apply the same discipline to OpenFlow length-delimited structures: validate before subtracting the fixed header, report malformed length, and terminate the enclosing parse instead of underflowing or revisiting the same offset.

- **Wiretap format detection must respect streamability.** Guy Harris's merged !5898 separates provisional pcap classification from final variant selection. Packet-record heuristics that require lookahead/rewind are used only for seekable input; pipes avoid probes that could wait for future records. Final subtype and timestamp semantics are assigned after the variant is known.

- **Define packet-count options by pipeline stage.** In merged !5867, John Thacker distinguishes TShark `-c` (packets read) from `-a packets:` (packets written after display-filter/dependency handling). Documentation and counters should state exactly which processing stage advances the count.

- **Reuse registered subdissectors and split independent fixes.** Merged !5902 invokes the existing ISAKMP dissector for EAP-IKEv2 instead of duplicating the parser. A separate ISAKMP tree-length correction is moved to !5917 so it can be reviewed and backported independently. The MR also supplies a representative capture after reviewer request.

- **Initialize third-party state objects as complete objects when later library/error paths can inspect members you did not explicitly assign.** Gerald Combs's merged !5910 replaces selective zlib stream initialization with whole-object zero initialization after Coverity identifies a potentially uninitialized member.

## Corroborating review/tooling evidence

Merged !5876 contains direct João Valverde guidance to retain Wireshark's `ws_strdup_printf()` wrapper rather than replace it with the superficially similar GLib helper, citing native-I/O behavior, optimization, and stricter checking. The same MR has Gerald Combs explicitly exclude a Windows-only source from a clang-analysis configuration that cannot compile Windows headers. !5894 additionally shows Windows CI catching a narrowing warning missed elsewhere.

!5878 and !5861 reinforce the existing submission rules for component-prefixed commit subjects, topic branches, and maintainer-collaboration-enabled forks.

## Evidence weighting

The strongest evidence in this batch is !5898 (Guy Harris), !5910 (Gerald Combs), !5867 (substantive John Thacker review), !5900 (Jaap Keuter), and merged malformed-input fixes !5885/!5862/!5863. All 50 MRs in the batch merged.
