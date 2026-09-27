# Durable conventions from !7461-!7510

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

## Wire layout must come from protocol semantics, not C struct packing

When a native struct exists only to calculate the size or layout of an on-wire block, remove that dependency rather than wrapping the struct in compiler-specific packing attributes. In !7470 Stig Bjørlykke explicitly said Wireshark should not depend on struct pack sizes; merged !7472 follows through by replacing `sizeof(sample_set_t)` with the protocol's explicit 34-byte block size. This strongly corroborates the later Guy Harris wire-array guidance recorded elsewhere in the notebook.

## User-configurable bit partitions need a whole-field invariant

If preferences divide a fixed-width wire field into variable-width subfields, validate the complete partition before decoding. Merged !7503 checks that all four O-RAN eAxC widths are nonzero and sum to exactly 16, emits expert information when the configuration is inconsistent, and uses bit-offset extraction APIs instead of rewriting static masks. A dynamic field layout is a runtime contract, not merely four independent integer preferences.

## Store UAT values in their semantic type

Do not store inherently numeric UAT configuration as strings merely because text is how the dialog presents it. Jaap Keuter challenged the string representation in !7468; merged follow-up !7471 stores ports as integers and uses `UAT_DEC_CB_DEF`, eliminating manual parse/copy/free paths. Choose the UAT callback/storage type that represents the configuration domain.

## A new dependency API and the declared minimum version must move together

!7468 used `ares_set_servers_ports()`, and review immediately caught that Wireshark's declared c-ares minimum was older than the API. !7482 raises and documents the support baseline. When master adopts an API introduced after the current minimum dependency version, update the minimum, CI/platform baseline, and release documentation coherently; stable branches can intentionally retain the older dependency contract.

## Bridge event loops without moving callback ownership accidentally

Merged !7499 offloads only the blocking GLib poll to a worker thread. Readiness is handed back to Qt, and `g_main_context_dispatch()` runs on the UI/main thread so existing GLib callbacks retain their expected thread context. The bridge also detects when Qt already supplies GLib integration and declines to install a duplicate loop. Foreign-loop integration should separate blocking wait mechanics from semantic callback ownership.

## Stateful reassembly tests should cover pathological order, overlap, retry, and redissection

Merged !7479 builds a regression corpus from real QUIC captures covering single-packet fragmentation, cross-packet fragmentation, out-of-order delivery, retransmission/retry, overlaps, duplicate data, and a retry where an original packet is missing. The test is run in normal and two-pass (`-2`) dissection and checks warning-level expert output. Reassembly changes deserve a matrix of arrival/state pathologies rather than a single happy-path sample.

The same MR also documents a layered responsibility boundary: QUIC reorders CRYPTO bytes, while TLS may independently reassemble the in-order handshake record. A tree that visually shows two levels of fragmentation can be correct if it reflects the protocol layers' actual responsibilities. Do not collapse layers merely for presentation unless the alternative API provides a real semantic benefit.

## Shared decompression belongs below individual dissectors, with a hostile-output budget

In !7494 John Thacker explicitly favors moving Snappy raw-buffer access from individual dissectors into shared tvbuff helpers so all Snappy users can reuse one implementation. Review also points out that the uncompressed length comes from untrusted data and can drive a large allocation; shared decompression code therefore needs a defensible output-size/resource policy rather than letting every dissector reinvent one. The focused HTTP Snappy capture supplied in the MR is also appropriate regression evidence.

## Generated hidden fields are useful for filterable computed metadata

Merged !7466 exposes the Signal-PDU's configured/computed PDU name as a generated hidden field. This provides a stable display-filter/column value without duplicating information visibly in the packet tree. For semantic metadata that is not backed by a literal packet byte range, a generated field is appropriate; hiding it can keep the tree uncluttered when the primary purpose is machine/filter access.

## Generated build artifacts need explicit target dependencies

Merged !7491 creates a real CMake target for generated WSLua registration files and makes lrexlib depend on it. If one build target consumes a generated header/source from another step, encode that order explicitly in the build graph rather than relying on incidental include paths, directory layout, or parallel-build luck.

## Submission/testing corroboration

Merged !7473 records João Valverde's guidance that contributors need not manually rebase an MR unless there is a merge conflict; needless rebases create review churn. !7478 again records Alexis La Goutte requesting a focused pcap and squashed history. Closed !7500 is explicitly superseded and is not treated as positive design precedent.
