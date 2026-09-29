# Wireshark MR review findings 5511-5560

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Evidence weighting favors merged master work, accepted maintainer guidance, and later corrective/successor MRs over closed or superseded proposals.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !5560 | merged | Deep | John Thacker removes duplicated timestamp-format state from text_import and uses the caller-owned format directly; ISO selection uses g_strcmp0. |
| !5559 | merged | Discussion-focused | NorDig descriptor support. Anders Broman asks that the MR, and the contributor's other MRs, be squashed to a single commit. |
| !5558 | merged | Deep | John Thacker moves text2pcap option failures onto common cmdarg_err messages and project exit codes instead of ad-hoc stderr/EXIT_FAILURE. |
| !5557 | merged | Deep | John Thacker reduces text_import globals by using text_import_info_t directly and returns packet counters through that shared parameter object. |
| !5556 | merged | Discussion-focused | Separates BICC field registrations/value semantics from ISUP where shared registrations produced misleading output; Alexis La Goutte also requests typo cleanup and squashing. |
| !5555 | merged | Deep | John Thacker converts reusable text_import parsing from process exit paths to import_status_t propagation plus project reporting; repeated time parse failures get bounded user-facing warning behavior. |
| !5554 | merged | Discussion-focused | Extcap configuration validation before capture. Dario Lombardo and Roland Knall discuss the historical HAVE_LIBPCAP coupling and whether extcap-only capture should remain possible. |
| !5553 | closed | Discussion-focused | Superseded multilingual DVB descriptor attempt. Alexis requests a dead-store fix and a rebase/squash; not accepted as implementation precedent. |
| !5552 | closed | Scanned | Accidental earlier duplicate of the multilingual descriptor work; author closes it. No accepted implementation precedent. |
| !5551 | merged | Scanned | Gerald Combs sorts Qt CMake file lists, fixes indentation, and removes a duplicate item. |
| !5550 | merged | Deep | John Thacker adds ISO-8601 text-import timestamps by reusing iso8601_to_nstime and making the GUI accept the ISO mode. |
| !5549 | merged | Deep | John Thacker centralizes text-import write/scanner failures in the shared import layer, includes input/output names in diagnostics, and normalizes return status. |
| !5548 | merged | Deep | John Thacker finishes text2pcap integration with common command-line and report-message callbacks so shared import code can report through the frontend. |
| !5547 | merged | Scanned | Show/Export Packet Bytes is enabled only when the selected field has a data source and positive byte length, excluding generated/zero-length fields. |
| !5546 | merged | Scanned | Gerald Combs decouples generic Qt ColorUtils/StockIcon helpers from the Wireshark application subclass by using qApp. |
| !5545 | merged | Deep | John Thacker ports text2pcap's hex+ASCII lookback so ASCII-column text that resembles hex bytes is not accidentally parsed as more packet data. |
| !5544 | merged | Deep | Jaap Keuter centralizes VARINT handling around ENC_VARINT_MASK; unsupported encoding is treated as a dissector-programming invariant, not packet malformation. |
| !5543 | merged | Scanned | Updates WiX discovery paths from 3.10 to 3.11. |
| !5542 | merged | Scanned | Gerald Combs removes redundant manual Windows PATH injection where CMake/tool discovery already locates required programs. |
| !5541 | merged | Deep | John Thacker adjusts the lexer so a valid final hex byte at EOF does not require a trailing LF. |
| !5540 | merged | Deep | John Thacker fixes synthetic IPv6 payload length: the IPv6 base header is excluded from the payload-length field. |
| !5539 | merged | Scanned | Merged successor for DVB 8K/HEVC descriptor work; it outweighs closed !5513 as implementation precedent. |
| !5538 | merged | Deep | Extcap preferences now treat empty string as a real value and add an explicit reset-to-default action, rather than overloading empty to mean default. |
| !5537 | merged | Discussion-focused | Anders Broman adds a named clang-check exclusion for packet-PROTOABBREV.c after a false positive; later !5663 generalizes this into a source-class rule. |
| !5536 | merged | Scanned | Jaap Keuter renames ENC_VARIANT_MASK to ENC_VARINT_MASK so the identifier states the actual semantic domain. |
| !5535 | merged | Scanned | John Thacker adds IPv6 address/state support to text_import using an explicit IPv4/IPv6 union. |
| !5534 | merged | Scanned | John Thacker adds caller-configurable dummy IPv4 source/destination addresses to text_import. |
| !5533 | merged | Scanned | João Valverde replaces GLib-specific 64-bit constant macros with standard UINT64_C. |
| !5532 | merged | Deep | João Valverde makes Debian development packages consume the build system's installed public-header tree instead of duplicating hand-maintained header inventories. |
| !5531 | merged | Scanned | John Thacker removes platform-specific includes made obsolete by routing text2pcap output through the higher-level Wiretap API. |
| !5530 | merged | Deep | John Thacker renames pcap_link_type to wtap_encap_type because the value is a Wiretap encapsulation identifier, not a libpcap linktype. |
| !5529 | merged | Scanned | João Valverde exposes ws_version.h through the public wireshark.h umbrella and updates package installation. |
| !5528 | merged | Scanned | Fixes extcap selector syntax so default=true belongs on the selected value row, not the selector argument. |
| !5527 | merged | Scanned | Earlier extcap behavior treats empty stored selector preference as absent/default; later merged !5538 supersedes this for domains where empty is meaningful. |
| !5526 | merged | Deep | Jaap Keuter removes a GTK-era iterator-cookie workaround after the consumer that required it no longer exists. |
| !5525 | merged | Scanned | Jaap Keuter trims a duplicated protocol-tree API catalog from README.dissector and points readers to epan/proto.h for the complete interface. |
| !5524 | merged | Deep | Jaap Keuter replaces MySQL's illegal internal proto-tree API call with a registered field through public proto_tree APIs and marks the synthetic row number generated. |
| !5523 | merged | Scanned | Release backport of the IEC101 fixed-frame-length fix from !5521; no additional convention. |
| !5522 | merged | Scanned | Extcap selector widgets expose the argument tooltip. |
| !5521 | merged | Deep | John Thacker derives IEC101 fixed-frame PDU length from the configured link-address length instead of hard-coding the common value. |
| !5520 | merged | Deep | João Valverde adds ws_file_size_t/ws_file_ssize_t so file-I/O wrapper types match Windows _read/_write versus POSIX size_t/ssize_t signatures. |
| !5519 | merged | Deep | Adds PREF_PASSWORD: secret value is remembered only in process memory, never serialized to the preferences file, while still supporting runtime/UI/CLI configuration. |
| !5518 | merged | Deep | John Thacker replaces text2pcap's direct pcap writer with wtap_dumper, enabling common file formats/error paths and validating options before opening output. |
| !5517 | merged | Discussion-focused | Gerald Combs updates migrated wiki attachment URLs; review raises whether trailing whitespace should be caught automatically, but no new accepted checker behavior lands here. |
| !5516 | merged | Discussion-focused | João Valverde cleans printf formats; Stig Bjørlykke discusses preferring standard inttypes macros over remaining GLib format macros. |
| !5515 | merged | Deep | João Valverde splits regex matching into NUL-terminated and explicit-length APIs, removing SIZE_MAX as a magic semantic sentinel. |
| !5514 | merged | Deep / high-authority | João Valverde adds POSIX compatibility ssize_t support. Guy Harris explicitly notes Windows POSIX-like read/write/socket APIs return int; merged !5520 supplies exact wrapper types for that ABI. |
| !5513 | closed | Discussion-focused | First DVB 8K attempt; Alexis flags commit-message line length and the author elects to remake it. Merged !5539 is the authoritative successor. |
| !5512 | merged | Deep | Martin Mathieson and Pascal Quantin explicitly reject cosmetic edits to ASN.1 files copied from external specifications because regeneration would overwrite them; changes are removed. |
| !5511 | merged | Scanned | João Valverde fixes scanf conversions to use SCN* macros rather than PRI* output-format macros. |
