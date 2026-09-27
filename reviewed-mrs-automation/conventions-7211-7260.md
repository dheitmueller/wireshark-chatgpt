# Durable conventions from Wireshark MRs !7211–!7260

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are primary evidence; closed work is used only for negative/process lessons.

## Consume typed Wiretap metadata as typed metadata

Merged !7239, authored by Guy Harris, fixes packet-verdict dissection after Wiretap changed the pcapng verdict option from an opaque byte string into `packet_verdict_opt_t`. EPAN had continued to parse the old representation and crashed.

**Rule:** once Wiretap has parsed a capture-format option into a semantic typed object, higher layers should consume that object rather than reinterpret the original bytes. Keep capture-format byte parsing in Wiretap.

## Framing bytes are not nested payload protocols

Merged !7252 and stable backport !7257 fix HTTP chunk handling that called the generic data dissector once per chunk. Many chunks could exhaust `PINFO_LAYER_MAX_RECURSION_DEPTH`. The accepted code adds each chunk as `FT_BYTES` and dissects content only after dechunk/reassembly.

**Rule:** framing units that merely delimit a larger semantic payload belong as fields/subtrees. Invoke the content dissector at the semantic entity boundary after framing is removed.

## Numeric address objects carry host-order semantic integers

Merged !7228 made `AT_NUMERIC` width-aware, but review exposed byte-order ambiguity. Pascal Quantin flagged portable integer formatting; Brian Sipos and João Valverde argued that a generic numeric address should not impose little-endian storage. Merged follow-up !7235 establishes host byte order and converts openSAFETY values at the dissector boundary.

**Rules:** store generic numeric semantic values in host order; convert from wire/network order at the protocol boundary. Preserve the full declared width throughout formatting/sorting; !7228 review caught a Qt `QString::toInt()` path that would overflow 64-bit values. Use portable integer-format macros such as `PRIu64`.

## Length-bearing text must remain length-bearing

Merged !7212 adds display-filter literal strings containing embedded NUL bytes. The implementation extends `wmem_strbuf`, escaping helpers, fvalue accessors, and regex wrappers to carry explicit lengths.

**Rule:** if NUL is legal data, every storage, escape, compare, and regex API in the path must carry length separately. A single fallback to C-string termination silently truncates the semantic value.

## Generator text I/O must be encoding-explicit

Merged !7248 makes WSLua generators and the AUTHORS generator specify UTF-8 for file and subprocess text I/O. The surrounding Perl-to-Python series (!7238, !7240, !7256, !7259) treats generated-output parity and intentional whitespace/newline changes as reviewable behavior.

**Rules:** repository generators should not inherit locale-dependent encoding. Use explicit UTF-8 for files and text subprocess streams. When replacing a generator implementation, compare semantic output and document intentional textual differences.

## Use typed Qt signal/slot connections

Merged !7224, authored by Gerald Combs, fixes an incorrect QComboBox signal assumption and a duplicate connection while migrating touched code to typed member-pointer connections.

**Rule:** prefer new-style typed `connect()` syntax so signal/slot signature mismatches become compile-time errors.

## Central type predicates define type families

Merged !7213 adds `FT_UINT_STRING` to `IS_FT_STRING()` and removes one-off exceptions from fvalue string accessors.

**Rule:** encode semantic type-family membership in the central predicate. Do not scatter `|| type == ...` exceptions across generic APIs.

## Keep generated dissectors and template source synchronized

Merged !7245 fixes a duplicate X.509 field abbreviation in the ASN.1 template and regenerated dissector; !7250 backports it.

**Rules:** fix generated dissector defects in the template/generator source and regenerate the derivative. Registered filter abbreviations are identities, not merely visible labels.

## Recover from recognizable protocol defects with expert diagnostics

Merged !7249 handles common PTP implementation mistakes while warning through expert info.

**Rule:** when a deployed nonconformance is recognizable and the intended interpretation remains bounded, preserve useful dissection but expose the deviation; do not silently normalize it.

## Weight later accepted architecture over older merged experiments

Merged !7255 removed an unresolved display-filter syntax type. Later accepted work already recorded in this notebook, including !13144, reversed that direction.

**Rule:** merged status is not timeless policy. When later merged architecture deliberately supersedes an older design, keep the old MR as context and weight the newer accepted direction more heavily.

## Closed work: unexplained integration-sentinel edits are not acceptable

Closed !7231 changed `nnn` without rationale. Guy Harris warned that it would probably break Transifex and UI translations.

**Submission rule:** require a clear integration rationale and downstream validation for changes to localization/build sentinel files.
