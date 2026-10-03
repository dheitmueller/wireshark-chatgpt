# Review findings: Wireshark MRs !258-!308

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 valid MR records, descending from !308 toward !258 and skipping zero-byte corpus artifact `mr_261.json`. Merged master MRs are weighted above stable-branch backports and closed predecessors.

## Durable findings

### !300 — Validate packet-derived persistent configuration state

Martin Mathieson fuzzed the new IDN dissector and showed that a malformed configuration packet could poison state used by later packets. A fuzzed zero `sample_size` reached a later division and caused SIGFPE. The merged revision restored a defensive check.

**Rule:** values learned from earlier packets remain untrusted packet input. Validate invariants before retaining configuration state and before later division, allocation, indexing, or loop use. Stateful fuzzing should mutate the packets that establish state as well as those that consume it.

### !289 — Preserve mandatory lower-layer dispatch

Jaap Keuter rejected a condition that could suppress the follow-on dissector and emphasized that the R-Tag payload must be handed off. The merged code always constructs the EtherType context and calls the next dissector. The same review removed irrelevant preference/template baggage and caught a falsely unused pseudo-header parameter.

**Rule:** once an outer header establishes encapsulated payload and its discriminator, unrelated reserved/configuration conditions must not accidentally block mandatory handoff. Audit copied dissector templates for semantic relevance.

### !275 — TvbRange offsets belong to the range

The merged WSLua fix validates offset/length against the current `TvbRange`, computes omitted length from that range, and adds the range's backing offset only at final TVB access. Stig Bjørlykke requested explicit tests for `raw(offset)` and `raw(offset,length)`; the MR adds a broad matrix of combinations.

**Rule:** bounded-view APIs use the view's coordinate system and translate to backing storage only at the access boundary. Test all optional offset/length combinations for nested views.

### !272 — One conversation can contain multiple protocol associations

PROFINET previously reused one station object when multiple ARs shared the same device conversation. The merged code records AR UUID, input/output frame IDs, and setup/release frame numbers to select the correct active state. Pascal Quantin also required proper visited handling, NULL checks, and allocator-lifecycle cleanup.

**Rule:** transport/MAC conversation identity is not always the final persistent-state key. Multiplexed protocol associations require association-specific identity and validity intervals.

### !281 — Pin cross-implementation derived identifiers with deterministic vectors

Community ID merged with a maintained capture plus exact baseline outputs and filter tests based on the same externally specified behavior used by other implementations. Guy Harris also challenged byte-order handling of 8-bit ICMP values, reinforcing explicit width/serialization reasoning.

**Rule:** derived identifiers intended to interoperate across implementations should have exact regression vectors that pin the externally observable value.

### !277 — Raw byte fields do not have integer endianness

Alexis La Goutte requested `ENC_NA` rather than `ENC_LITTLE_ENDIAN` for `FT_BYTES` fields.

### !276 — Normalize tolerated syntax at the wrapper boundary

WSLua Base64 now pads unpadded input in a private buffer before calling GLib's decoder. Tests cover both padded and unpadded forms.

**Rule:** if a public wrapper intentionally accepts broader syntax than a lower-level helper, normalize to the helper's contract before delegation and test both canonical and tolerated forms.

### !296 / !302 / !301 — Keep generator source and generated output synchronized

Guy Harris's NCP master fix updates both `tools/ncp2222.py` and the generated dissector include; stable backports carry the same correction. This strongly corroborates the notebook's generator-workflow rule.

### !292 — Pass the discriminator the consumer actually needs

Guy Harris fixes NCP by passing the NDS class-definition type rather than unrelated NDS flags. The field is obtained with `proto_tree_add_item_ret_uint()` and then drives later parsing.

**Rule:** propagate the semantic selector required by the downstream contract, and prefer returning tree-add APIs when one wire value is both displayed and consumed.

### !303 / !304 — Commit validation may enforce maintainer-edit permission

Gerald Combs synchronized commit validation to require the MR collaboration setting so maintainers can rebase and make minor edits. This corroborates existing submission guidance.

## Corroborating and lower-impact findings

- !305 corrects !284's packet-diagram use of `FT_NONE` representation text so synthetic gap items with no field metadata get a safe default instead of being dereferenced.
- !282 avoids rebuilding a hidden packet-diagram scene; !266 fixes ownership of a heap-allocated Qt `FieldInformation`.
- !286 is the accepted IPP-over-USB submission; closed !285 is down-weighted. Martin Mathieson requested meaningful references/comments and visibility for duplicated definitions.
- !268 supplies focused GQUIC ACK reproducer captures; !271, !279 and !280 are stable backports.
- !274 moves MQ message state out of process globals into the per-dissection parameter object and makes a helper report bytes consumed, corroborating existing state-scope and parser-progress guidance.
- !288 with !297-!299 fixes GSM subtree placement.
- The NCP stable-branch MRs are primarily corroborative; master implementations are weighted more heavily.
- !285, !270 and !269 are closed/unmerged and are not accepted implementation precedent.

No SMPTE ST 291/VANC packet type was encountered.
