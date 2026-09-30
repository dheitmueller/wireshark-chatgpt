# Durable conventions from !4061–!4110

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Make intentional generic fallback explicit in subdissector schemas
Merged !4104 originally inferred omitted Thrift fields from field-number ordering. Jaap Keuter objected because Wireshark must remain useful when protocol data is incorrect. Anders Broman proposed an explicit descriptor value, and the merged code adds `DE_THRIFT_T_GENERIC`, allowing a declared field position to be intentionally delegated to generic Thrift dissection while preserving malformed/unordered-field checks.

**Rule:** when a structured subdissector intentionally leaves part of a schema to a generic decoder, encode that intent explicitly in the schema/API. Do not infer fallback from a condition that can also be produced by malformed wire data.

## Scope expensive global dissection modes to the operation that needs them
Merged !4101 exports `epan_set_always_visible()` so C plugins can force full field visibility. Roland Knall calls out the large performance cost and prefers deeper dissection only around the tap/redissection lifecycle that needs it.

**Rule:** an expensive global dissection mode must not become a plugin-presence side effect. Enable it for the shortest lifecycle that requires it and restore ordinary visibility.

## IANA assignment is evidence, not automatic permission to broaden default dispatch
In merged !4096, John Thacker initially proposed adding a second IANA-registered UDP port for AVTP. Anders Broman raised the practical false-positive risk. The final MR retains the protocol correction but removes the new default binding.

**Rule:** default port bindings should balance standards registration with recognizer strength and real false-positive risk.

## Checker warnings are structural leads that still need local semantic review
Merged !4094 shows Martin Mathieson using `check_typed_item_calls.py --consecutive --file ...`; he explains CI hard-fails only clear errors so warning-only findings still need human judgment.

**Rule:** inspect checker warnings even when CI is green and decide them against actual protocol semantics.

## Choose buffer integer domains from the narrowest real I/O contract
Merged !4085, authored by Guy Harris, caps Wiretap reader buffers at 2^30 and uses `guint` because Windows `_read()` and zlib expose narrower integer interfaces. Closed !4081 and !4082 reinforce the same rationale.

**Rule:** choose the integer domain from the complete external API chain and impose bounds that make every conversion representable. Wider host types do not make a narrower backend API capable of larger transfers.

## Persisted preferences remain compatibility state when removed
In merged !4075, Jaap Keuter objects to deleting an O-RAN preference because existing preference files then produce read errors. The accepted code registers the old key as obsolete.

**Rule:** when deleting or replacing a preference users may have persisted, keep an obsolete or migration path; development-build exposure can still create real persisted state.

## Derive executable paths from the target actually being invoked
Merged !4062 replaces handcrafted `WS_PROGRAM_PATH` with CMake `$<TARGET_FILE_DIR:tshark>`. Merged !4074 tightens the test path to `$<TARGET_FILE_DIR:wmem_test>`.

**Rule:** derive runtime paths from the concrete target whose executable is needed; do not reconstruct platform/configuration directories or assume another target is colocated.

## Child parsers that consume parent framing must update parent ownership state
Merged !4071 is the master-origin RTCP padding fix later corroborated by !4388. The transport-feedback child consumes padding and clears the parent's padding flag.

**Rule:** when a child decoder consumes framing the parent would otherwise process, explicitly transfer that ownership in shared parser state before returning.

## Prefer protocol-domain resource bounds to arbitrary emergency caps
Merged !4078 removes an arbitrary HID report-descriptor count cap while adding bounds derived from actual usage/page domains.

**Rule:** where practical, constrain packet-driven allocation with protocol-defined cardinality/range limits rather than unexplained magic maxima.
