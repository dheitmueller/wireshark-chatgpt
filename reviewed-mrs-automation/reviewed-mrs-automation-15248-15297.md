# Wireshark MR review automation — 2026-09-20

Model: GPT-5.6 Sol

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

## Selection and tracking

Before selecting this batch, the already-reviewed set was reconstructed from all available review tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the aggregate automation tracking, and the per-run files under `reviewed-mrs-automation/`. The historical !17571-!17620 batch remains explicitly preserved and counted. Candidate MR numbers were compared against the exact reviewed set; no numeric range was assumed complete merely because some members appeared in a ledger.

The immediately preceding exact run reviewed !15347 through !15298. The fifty highest-numbered valid corpus MRs not already present in the reviewed set are therefore !15297 through !15248 inclusive. All 50 records were individually inspected, including available discussions and diffs.

## Exact MRs reviewed in this run

!15297, !15296, !15295, !15294, !15293, !15292, !15291, !15290, !15289, !15288

!15287, !15286, !15285, !15284, !15283, !15282, !15281, !15280, !15279, !15278

!15277, !15276, !15275, !15274, !15273, !15272, !15271, !15270, !15269, !15268

!15267, !15266, !15265, !15264, !15263, !15262, !15261, !15260, !15259, !15258

!15257, !15256, !15255, !15254, !15253, !15252, !15251, !15250, !15249, !15248

Count: **50 MRs**.

Status weighting: **43 merged**, **7 closed/unmerged** (!15296, !15285, !15283, !15280, !15274, !15251, and !15250). Closed proposals were down-weighted relative to accepted successors. In particular, !15296 and !15280 are unsuccessful MACsec submission forms relative to accepted !15299; !15285 was superseded by accepted 802.11 work including !15288; and !15250/!15251 were superseded by merged !15256. !15274 was useful negative review evidence because Gerald Combs identified that its warning-driven change misunderstood the custom field-format callback's documented result-buffer contract.

## Durable findings promoted to the notebook

### Give pcapng option padding exactly one owner and reuse body-size calculations

Merged master !15263, authored and merged by John Thacker, fixes pcapng hash/verdict option length bugs, including omitted type-octet accounting, a wrong option type, and unsafe handling of fixed-length hashes. Guy Harris explicitly challenged whether any `pcapng_compute_..._option_size()` helper should add alignment padding. Merged follow-up !15271, again authored by John, generalizes the answer: option sizers report the option-body length while the enclosing block-size and write layers own padding. That allows the same helper to drive both aggregate block sizing and the option header's `value_len`, eliminating duplicate formulas. Added to `capture-option-length-budget-conventions.md`.

Notebook commit: `69bb8156d783c0a2c97fe08b193930ff7beb0b1c`

### Propagate lower-level API failures through wrappers until callers can react

Merged master !15261 adds `gcry_md_open()` failure handling through security/digest helpers. Guy Harris specifically noted that a helper which notices failure but returns no status cannot let its caller respond, and requested a success/failure indication. The accepted implementation propagates the failure upward rather than proceeding with an unusable digest context. Added to `c-api-call-contract-conventions.md`.

Notebook commit: `2288827509c5a46a07fd7b94a67cb27117b7bf55`

### Compile-test installed public headers from a downstream-style consumer

Merged master !15265 adds `<epan/dfilter/dfilter.h>` to Debian's `headers-check.c`, explicitly to catch downstream development-header failures such as Debian #1068410. A normal Wireshark build can hide missing dependencies or include-order assumptions that become visible only through the installed public include surface. Added to `checker-target-conventions.md`.

Notebook commit: `c4efa27d25f1a5a23e999cad5d3c75605880e968`

### Random Wiretap reads must not mutate the sequential-read cursor

Merged master !15289 fixes BLF random access performed in the middle of a linear scan. Random reads had updated `current_real_seek_pos`, which represents the persistent sequential scan position, so resuming the linear read could proceed from incorrect bookkeeping. The accepted fix advances that state only for non-random reads. Added as `wiretap-read-cursor-conventions.md`.

Notebook commit: `e577b522d90f07ac404055e76de59cd3513852f7`

### Apply resource limits according to block semantics, not merely shared framing

Merged master !15281 fixes dumpcap's pcapng pipe-length validation. `cap_pipe_max_pkt_size` is a packet/record-oriented limit, but the old code applied it to every pcapng block and could reject legitimate large metadata blocks. The accepted implementation classifies data-producing block types and applies the packet limit only to those blocks. Added as `capture-block-size-validation-conventions.md`.

Notebook commit: `a6f6fc39f5d73f372c8d3d28121059790733fe7a`

## Strong corroborating or contextual evidence retained without duplicate rules

- **!15297, !15267, and !15258** independently reinforce the existing logging convention that `config.h` comes first and `WS_LOG_DOMAIN` must be defined immediately afterward, before a header can transitively include `wslog.h`. The notebook already contains this rule in `logging-conventions.md`, so no duplicate was added.
- **!15278**, authored and merged by John Thacker, reinforces the existing partial-capture rule that capture start is not conversation start. A TCP capture can begin with SYN-ACK, and conversation-analysis facts that define identity should be established from strong first-pass protocol evidence rather than appearing only during redissection.
- **!15284, !15286, and !15287**, authored and merged by Guy Harris, provide strong additional evidence for maintaining generated X11 dissector artifacts from their authoritative generator/provenance inputs rather than treating generated output as independent source.
- **!15294** shows that follow-filter callbacks requiring `epan_dissect_t` context must receive that full context rather than a reduced substitute such as `packet_info`; otherwise protocol-specific state such as SIP Call-ID association can be lost.
- **!15273** reinforces context-sensitive state mutation: seeing the same ASN.1 type is not sufficient to update PDCP-NR security state unless it appears in the semantic `SecurityConfigSMC/securityAlgorithmConfig` context that actually changes that state.
- **!15264 and !15252** reinforce initialization/nullability guidance in generated MMS custom code. !15264 replaces partially initialized transaction allocations with `wmem_new0`, and !15252 guards transaction state exposed by fuzzing before dereference.
- **!15262** records useful checker-review nuance from Martin Mathieson: duplicate field abbreviations are normally suspicious because the checker is meant to catch copy/paste mistakes, but deliberately shared filter identity can be valid if the fields are semantically intended to always filter together.
- **!15259**, authored by John Thacker, extends SIP transaction matching beyond UDP and adds transport type to the hash key. Its discussion distinguishes request/reply association from transport-layer retransmission handling; the state identity should include dimensions needed to prevent cross-transport collisions even when some analysis behavior is transport-specific.
- **!15256** is the accepted PIM RFC 9465 implementation after closed !15250/!15251. Review requested both representative pcaps and resolution of an uninitialized-variable compiler warning; the merged successor supplied two captures and an explicit unknown-type path, reinforcing the value of test evidence plus warning cleanup before submission.
- **!15255** shows a dependency-compatibility case where a deprecated c-ares API cannot simply be eliminated based on compile-time version: thread support is optional and discoverable only at runtime, so the code may need a fallback. This is useful evidence for capability-based dependency handling rather than version-only assumptions.
- **!15253** adds Linux's historical VXLAN UDP port 8472 alongside the IANA port 4789 only after review checks that Wireshark has no competing dissector claiming that port. This reinforces auditing dispatch conflicts before claiming a widely used nonstandard/default port.
- **!15248** correctly keeps payload offset and payload length in sync when an SDAP header is consumed from the main PDCP-NR tvbuff, while avoiding a second subtraction when deciphering already removed it; this is a straightforward local cursor/length invariant rather than a new notebook-wide rule.
- Closed **!15274** was deliberately not promoted: the compiler-warning-driven patch assumed the wrong buffer-size contract for a `BASE_CUSTOM` field formatting callback, and Gerald Combs pointed out that the callback receives `ITEM_LABEL_LENGTH`. The review lesson is already covered by the notebook's API-contract guidance: verify the actual API contract before changing code merely to silence a diagnostic.

## Continuation

The corpus is not exhausted. `mr_15247.json` exists and contains a valid merged MR record at the same corpus commit, so the next review run can continue backward from !15247. No scraper-restart notification is needed for this run.