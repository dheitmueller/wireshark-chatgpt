# Durable Wireshark conventions from MRs !4111–!4160

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Checker applicability must match the semantic source class

Merged !4156, authored by Guy Harris, excludes `idl2wrs.c` from dissector-oriented pre-commit checks because it is a dissector generator: checker pattern matching sees snippets embedded in generator format strings and can falsely treat them as real declarations. A checker should analyze only source classes for which its assumptions are valid. This is an early high-authority precursor to later generalized template/generated/example exclusions.

## Decide API visibility deliberately

In merged !4154, Guy Harris explicitly asks whether a helper is file-local, libwiretap-internal, or public. File-local helpers should be `static`; genuinely public Wiretap APIs require the project export annotation and symbol-manifest entry. John Thacker made the new helper static and kept the already-public `wtap_get_compression_type()` as the external facade. API scope should be a deliberate contract, not an accident of linkage.

## Validate fixed fields required by the selected block subtype

Merged !4144, authored by Guy Harris, checks that a pcapng Netflix “skip” custom block has room not only for the generic custom-block prefix but also for its subtype-specific 32-bit value before reading it. Generic framing validation does not prove that a selected subtype's fixed body is present.

## Authoritative per-packet metadata outranks fallback configuration

Merged !4150, authored by John Thacker, replaces the Ethernet FCS Boolean preference with heuristic/never/always modes but only consults that preference when Wiretap did not provide a definite FCS length. A preference or heuristic may fill an unknown; it should not contradict authoritative capture metadata.

## Prefer explicit owner scopes over ambient packet scope

Merged !4127 makes `next_tvb_list_t` retain the allocator that owns the list and uses that allocator for every child item. Merged !4124 and !4123 use the protocol tree's pool when the tree is the actual owner, while !4137 explicitly frees short-lived temporary allocations instead of putting them in a global packet arena. Merged !4136 additionally demonstrates that callback/lifecycle timing matters: the credentials tap must use epan scope because live-capture initialization can occur when file scope is not active.

## Stateful subdissection must not depend on tree construction

Merged !4115, authored by John Thacker, removes an `if (tree)`-style gate around MP2T subdissection because fragmentation state must be built on the first pass even when no display tree is requested. Presentation can be optional; semantic state construction cannot be skipped merely because the caller is not rendering the tree.

## Include local layer-instance identity when one frame can contain repeated dissector instances

The same !4115 change stores MP2T packet-analysis data under `pinfo->curr_layer_num`. AVTP can carry multiple MPEG-TS packets in one physical frame, and each child TVB can reuse local offsets, so frame number plus offset is not enough to distinguish all reassembly/analysis instances. Repeated encapsulation requires an instance discriminator.

## Sequence loss is not malformed syntax

Merged master !4116, authored by John Thacker, changes MPEG-TS continuity-counter loss from `PI_MALFORMED` to `PI_SEQUENCE`. Missing or discontinuous progression is a sequence diagnostic when the packet itself remains structurally valid. Later stable !4474 corroborates this master-origin rule.

## Generated dissector fixes belong in the authoritative source

Merged !4134 directly edited generated `packet-sv.c`. Guy Harris explicitly corrected the approach: edit the ASN.1 template and regenerate with the ASN.1 target. Guy-authored merged !4202 later supplied the authoritative implementation. Even when a direct generated-file edit expresses the right semantic idea, it is not the durable maintenance surface.

## Batch Qt model mutations when intermediate notifications trigger expensive work

Merged !4135 adds complete UAT rows in one model insertion rather than emitting many `setData()` / `dataChanged` notifications. In IO Graphs those intermediate notifications could repeatedly trigger redissection. When an operation is logically atomic and observers perform expensive work, expose it to the model/view layer as one mutation.

## Preserve registered display-filter compatibility when possible

Closed !4118 and !4122 contain lower-weight but direct maintainer guidance from Graham Bloice and Roland Knall: replacing registered filter fields can break saved filters and user workflows. Prefer adding a new field while retaining the old compatibility surface, with clear migration/deprecation guidance when renaming is unavoidable. Stronger later merged review remains authoritative for current compatibility policy.
