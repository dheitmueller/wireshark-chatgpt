# Durable conventions from Wireshark MRs 4811–4860

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Internal library API and external plugin API are different compatibility domains

Merged !4825 documents Wireshark's distinction between the public symbols used by Wireshark programs and the narrower external API intended for plugins. An exported shared-library symbol is not automatically a promise of long-term third-party source/ABI stability. The documented plugin ABI goal at this point was best-effort compatibility across micro/patch releases, while major.minor releases can require plugin recompilation. The Lua API has historically been treated as a stronger compatibility surface.

**Rule:** review API changes against the compatibility domain they actually belong to. Internal shared-library API, external C plugin API/ABI, and scripting APIs can have different stability policies even when they coexist in the same libraries.

## A retained pointer must live as long as the state retaining it

Merged !4841 fixes IDMP code that allowed packet-scoped `protocolID` storage to escape into longer-lived ROS state. The accepted low-risk fix duplicates the value into epan scope; the source also notes that session-owned state would be a better final architecture.

**Rule:** when a pointer is inserted into state with a broader lifetime, either copy it into storage whose lifetime covers that state or redesign the state object to own it at the natural protocol/session scope. Scope-managed allocation does not make cross-scope borrowing safe.

## Logical length and actual allocated capacity are separate invariants

Merged !4851 adds explicit allocation-size tracking to C12.22 alongside the logical element length and refuses a copy when the logical read would exceed the bytes actually allocated, independently of the destination bound.

**Rule:** when transformed or inconsistent data can make a logical/declared length diverge from physical storage, carry both values and validate each raw copy against the source allocation as well as destination capacity.

## Parser failure must be meaningful to the caller

Merged !4815 and stable !4823 attempted to stop repeated BT-DHT parsing on a zero-length element by reporting the condition and returning the number of bytes remaining. Later merged !5280 is authoritative: the caller interprets a positive return as successful consumption, so the correct structural-failure result is the helper's failure sentinel.

Merged !4818 supplies the complementary pattern: CBOR sequence parsing stops the outer repetition immediately when the child parser reports failure, and its fixture generator was extended to accept raw input.

**Rule:** define malformed-input returns together with the caller's advancement contract. A locally plausible positive value can be semantically wrong one frame up.

## Heuristic enablement requires parser robustness, not only recognition quality

Merged !4816, authored by John Thacker, states that BT-DHT's recognition test was strong enough, but the parser was not yet sufficiently robust under broad automatic input. Wireshark therefore kept the heuristic available while disabling it by default on the release branch.

**Rule:** default heuristic enablement requires both acceptable selectivity/cost and confidence in the full parse path. Explicit Decode As can remain available while the heuristic is opt-in.

## Give sibling variants explicit Decode-As identities instead of global mode state

Merged !4817, authored by John Thacker, separates BSSAP, BSSAP-LE, and BSAP into distinct registered identities/tables so SCCP Decode As and its UAT can select the intended variant. The implementation shares underlying code while putting variant/PDU state into packet-local proto data instead of a mutable global selector.

**Rule:** when related variants share implementation but users need explicit dispatch control, represent the variants in the dissector registry/table layer and carry the chosen variant in packet-local context.

## Self-contained subgrammars can establish invariants in their constructor

Merged !4832 replaces complicated display-filter lexer start states for slice/range syntax with one RANGE token and a handwritten `drange_node_from_str()` constructor. Integer parsing, endpoint-form parsing, positive-length checks, sign consistency, and ordering checks are performed before returning a range node.

**Rule:** when a compact subgrammar is easier and safer to parse as one semantic unit, a dedicated constructor/parser can reduce lexer/parser state complexity and make invalid objects unconstructible.

## Generated output is not an authoritative edit point

In merged !4857, Jörg Mayer noticed that the submitted Skinny change directly modified a file whose header says it is generated and will be overwritten. The contributor acknowledged the issue and later synchronized the generator/template path in !4906.

**Rule:** when a generated file names its source/template/generator, make the durable change there and regenerate.

## Re-evaluate predicates when changing the reference point of a recurrence

Merged !4853 changed RTP jitter/nominal-time calculations from a first-packet reference to a previous-packet recurrence to handle mid-stream clock-rate changes. John Thacker later identified a regression: `in_time_sequence` expressed plausibility relative to the first timestamp, not ordering relative to the previous packet, and the new recurrence could require a negative delta that the code did not model correctly.

**Rule:** when moving from absolute-baseline math to incremental/previous-sample math, re-audit predicates, signedness assumptions, wrap rules, and exclusion conditions whose meaning depended on the old reference point.

## Prefer Wireshark's portable shared time parser over platform-specific libc behavior

Merged !4820 adds text2pcap ISO-8601 timestamps by using `wsutil`'s `iso8601_to_nstime()` rather than assuming all supported platforms provide equivalent `strptime()` behavior.

**Rule:** for standardized timestamp syntax already modeled by a project utility, use the shared parser so tools inherit one cross-platform grammar and normalization behavior.

## Thread packet context through helper layers that need packet-aware behavior

Merged !4839 changes PVFS helper signatures so `packet_info *` reaches lower-level string/opaque-data parsing rather than passing a null context at an intermediate layer.

**Rule:** if a lower helper can report expert information, allocate from packet context, or otherwise depends on packet semantics, propagate packet context explicitly through intermediate helpers.
