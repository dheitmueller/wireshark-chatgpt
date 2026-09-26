# Review Findings: Wireshark !8261-!8310

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This batch contains 46 merged MRs and four closed/unmerged MRs (!8305, !8283, !8282, !8280). Merged master changes carry the most weight; stable backports corroborate accepted behavior; closed submissions are used only as qualified negative/superseded evidence.

## Strong durable findings

### Protocol string values and display formatting are separate domains (!8290, !8301; negative precursor !8283)

João Valverde's merged !8290 introduces `BASE_STR_WSP` specifically so whitespace normalization can affect the displayed representation of an `FT_STRING` without replacing the semantic field value. The MR explicitly calls the pattern `proto_tree_add_string(..., tvb_format_text_wsp(...))` problematic because it stores a presentation-formatted value and therefore changes display-filter semantics. Merged !8301 complements this by using `proto_tree_add_item_ret_display_string()` when a dissector needs the exact display representation for a column or label, avoiding a second formatting pass. Closed !8283 is useful precursor evidence but is not weighted as highly as the merged API work.

### Persistent analysis state must follow protocol scope; per-frame results belong on the frame (!8281)

John Thacker's merged !8281 moves IPFIX/NetFlow sequence-number state from a global observation-domain map into conversation data because RFC 5101/7011 define sequence tracking within a Transport Session. Results attached to a particular packet move to packet proto-data keyed by frame. The MR also records an important caveat: SCTP sequence semantics are per stream while a Wireshark conversation is per association, so the implementation is closer to correct but still lacks one discriminator. This is strong evidence that state keys must model the protocol's actual semantic scope, including multiplexing dimensions.

### Bounded escaping must reserve the worst-case expansion and terminator before consuming input (!8302)

Gerald Combs's merged !8302 fixes a Coverity-reported XML escaping overrun. The buffer limit is derived from the fixed buffer size and the longest entity expansion, with room for the terminating NUL, and the code flushes before the next input character could exceed that bound. The durable lesson is to reason in output-space, not input-space, when one input byte may expand to several output bytes.

### Never pass arbitrary or lookup-derived text as a printf-style format string (!8266; backports !8269-!8271)

Gerald Combs's merged master !8266 separates literal lookup strings from strings that intentionally contain format directives, then always calls `wmem_strdup_printf()` with an actual format string. The accepted stable backports repeat the same repair. A string that happens not to contain `%` today is still not a safe substitute for a format-string parameter unless the API contract explicitly says it is a format template.

### Domain-specific logging can be low-noise by default and promoted to fatal for targeted validation (!8284, !8286, !8291)

João Valverde's merged !8284 moves UTF-8 contract diagnostics to a dedicated `UTF-8` log domain at debug level because malformed packet-derived text was still common enough that a global warning was too noisy. Merged !8286 adds fatal-domain selection, and !8291 ensures a fatal domain is active even if ordinary level/domain filtering would otherwise suppress it. This creates a useful validation pattern: keep an operational diagnostic at an appropriate normal severity, but allow CI/fuzz/debug environments to make that domain fatal without globally increasing logging noise.

### A nested dissector entry point is part of the caller-data contract (!8262; backports !8272, !8273, !8275)

The GSMTAP LAPD fix switches from the generic `lapd` handle to `lapd-phdr` and calls it with an `isdn_phdr` using `call_dissector_with_data()`. The bug had existed since LAPD's dispatch interface was restructured. The durable rule is that choosing the right registered dissector name is not merely naming: different entry points can imply different required caller context.

### Fuzz driver loop state must be reset for every input selection (!8261)

Gerald Combs's merged fuzz-script fix clears `KEEP` and `PACKET_RANGE` at the start of each capture-selection iteration. Without explicit reset, shell variables from the previous large capture can affect later captures. Treat loop-local fuzz selection state as ephemeral even in shell, and reset it before deriving the next testcase configuration.

### Public symbol manifests must follow API/library movement (!8308, !8277; API-addition reminder !8307)

Merged !8308 moves the `format_text*` symbols from Debian's libwireshark symbol manifest to libwsutil after the implementation moved libraries, and normalizes the introduction version for the newly exported wsutil symbols. Merged !8277 repairs a missing exported symbol entry. !8307 leaves a source comment reminding maintainers that adding a shared `true_false_string` requires corresponding declarations and symbol-manifest updates. Public API work is incomplete until downstream ABI metadata matches the actual exporting library and symbol set.

### Review process: combine sample captures with compiler/checker cleanup (!8268)

The VITA-49 context-packet MR !8268 went through substantive review by Jaap Keuter and Alexis La Goutte. Review caught duplicate values/copy-paste errors, missing function prototypes, confusing all-zero field metadata, and mask-width warnings; the contributor also created an issue with a representative capture. This reinforces the established Wireshark expectation that new dissector functionality should arrive with realistic capture evidence and a clean static/checker result.

## Additional useful evidence

- !8310 replaces stringified structured lookup keys plus linked-list scans with a typed map keyed by SEID/address and reports over a 10x speedup on moderate captures; !8309 separately removes needless heap allocation for 32-bit scalar hash keys by using direct pointer encoding.
- !8292 moves generic text-formatting helpers from epan to wsutil and adds unit tests, reinforcing dependency-layering and test expectations for shared utilities.
- !8294 treats EtherNet/IP's UDP security transition like STARTTLS: after the protocol-defined response the existing flow becomes DTLS, while Decode As remains available for explicit user binding.
- !8287/!8274, authored by John Thacker, fix GTP optional-field presence checks by comparing the IE's declared length against the bytes actually required and report only the true undecoded remainder.
- !8276 shows review catching a copy/paste placement bug in a new vendor-specific 802.11 IE dissector; the contributor then audited sibling dissectors and submitted a separate cleanup.
- !8264 and !8288/!8289 continue the Qt migration to typed signal/slot connections so mismatches are caught at compile time.
- !8263 makes wmem string-buffer terminology explicit: logical length excludes the NUL, while allocated size includes storage for it.
- !8305 is a duplicate closed MR; !8280 was closed after Gerald Combs identified !8277 as the already-merged fix; !8282 and !8283 were unmerged refactor/prototype work and are not treated as final API design.
