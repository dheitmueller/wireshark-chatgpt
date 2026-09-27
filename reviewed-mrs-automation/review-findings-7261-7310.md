# Review findings: Wireshark MRs !7261–!7310

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Weighting: merged MRs are primary evidence; closed MRs are review/history evidence only. Maintainer comments are weighted by authority and specificity, with direct Guy Harris / John Thacker / Gerald Combs / Alexis La Goutte / other senior-maintainer guidance treated as especially significant when reflected in accepted code.

| MR | State | Weight | Review finding |
|---:|:---|:---|:---|
| !7310 | merged | high | SOME/IP UATs gain explicit reset callbacks for derived hash tables, better record-specific diagnostics, and checks against direct recursive self-reference. Invalid/reloaded configuration must not leave stale derived state behind. |
| !7309 | merged | medium | When the resolved-column checkbox becomes inapplicable/disabled, the UI also clears its checked state rather than presenting stale state as if meaningful. |
| !7308 | merged | medium | The Logwolf→Logray rename is carried through build targets, plugin paths, documentation, attributes, packaging, and application identifiers. Cross-tree product renames must be mechanically complete, not UI-only. |
| !7307 | merged | low | Corrects a GTP copy/paste display label so downlink data is not described as uplink. |
| !7306 | merged | high | New-dissector review required normal source layout (registration at the end), public registration prototypes, warning cleanup, focused capture evidence, release-note entry, and squashed submission. Gerald Combs also states that new dissectors normally land in major .0 releases; stable branches are for fixes. Later review exposed a packed-struct portability issue, reinforcing that green CI is not permission to depend on compiler-specific layout. |
| !7305 | merged | high | DCP-ETSI separates heuristic recognition from ordinary dissection, strengthens the heuristic from a two-byte to a four-byte discriminator, and registers the ordinary dissector for Decode As. Recognition policy and explicit dispatch are separate concerns. |
| !7304 | merged | high | STUN fingerprint verification is routed through `proto_tree_add_checksum()`, including status field and bad-checksum expert info, rather than hand-building checksum presentation. |
| !7303 | merged | high | KNX/IP replaces hard-coded registrations on one assigned and several unregistered ports with automatic range preferences defaulting only to the IANA-assigned port 3671. Common deployment ports are not sufficient justification for claiming unrelated registered/unregistered ports globally. |
| !7302 | merged | high | TP-Link Smart Home adds a cheap content recognizer (decrypted JSON prefix) before claiming traffic on port 9999, which is not assigned to this protocol. Positive sample compatibility was checked. |
| !7301 | merged | low | Removes an unused one-off source-rewrite tool that had not been needed since 2010; historical recovery belongs in version control rather than keeping dead tooling indefinitely. |
| !7300 | merged | medium | Coverity exposed a real copy/paste bug: guaranteed uplink bitrate calculation used the maximum-uplink variable. The accepted fix changes the data dependency rather than suppressing the analyzer. |
| !7299 | merged | medium | A shared callback-name variable used across registration worker/main threads is reset through the same mutex-protected setter used by other accesses, removing an unlocked write from the synchronization domain. |
| !7298 | merged | medium | PacketListHeader cleanup removes redundant cached state, validates the context-menu section index before preference access, and modernizes null use. View-derived state should be queried from the model/header rather than cached without need. |
| !7297 | merged | high | Column “can resolve” capability moves into a PacketListModel header role so the header does not need a separately propagated `capture_file *`. UI capability belongs with the model that owns the data dependency. |
| !7296 | merged | high | Packet-list sorting caches each expensive `columnString()` result once and reuses it for string and numeric comparisons. Guy Harris also challenged the vague “Improve” title, eliciting the actual speed/memory rationale. |
| !7295 | merged | medium | Packet-list helper declarations are centralized in `packet_list_utils.h` instead of being split across unrelated UI headers. Public helper placement should reflect subsystem ownership. |
| !7294 | merged | medium | Capture-abort paths use the single `exit_application(0)` abstraction rather than a duplicate main-window quit API, while still leaving through the main loop so registered cleanup runs. |
| !7293 | merged | low | Release-note wording/spelling cleanup only. |
| !7292 | merged | high | Once a valid STUN packet identifies the 5-tuple, the conversation is bound to the non-heuristic STUN dissector so later TURN ChannelData can be handled without weakening the heuristic. Weak message forms remain rejected during heuristic probing. |
| !7291 | merged | medium | Adds resolved/unresolved custom-column editing and keeps applicability logic consistent with existing saved custom-column state. Review emphasizes UI consistency across editing surfaces and avoiding controls that imply an unsupported operation. |
| !7290 | merged | medium | Perl→Python port makes the output path explicit, removing dependence on the script’s own directory/current working directory. Gerald Combs requests `#!/usr/bin/env python3` and executable mode for a project tool. |
| !7289 | merged | low | Removes stale documentation for a parameter that no longer exists. |
| !7288 | merged | medium | Conversation/endpoint dialog state is initialized in the shared traffic-table dialog and uses the CaptureFile accessor rather than reaching into raw display-filter storage. |
| !7287 | merged | low | Fixes `-Wdocumentation` by removing documentation for parameters no longer present in the constructor. |
| !7286 | merged | medium | File-private GRE bonding value tables are made `static const`, limiting symbol visibility to the translation unit. |
| !7285 | merged | medium | Removes duplicated row/current-row UI state and redundant packet-list wrappers; packet selection passes the actual `frame_data *`. Guy Harris catches a stale parameter in the documentation, showing API cleanup must include comments. |
| !7284 | merged | medium | Time-column reformat/redraw behavior moves from generic UI callbacks into PacketList methods that own the model/view behavior. |
| !7283 | merged | medium | Timestamp display changes resize the relevant PacketList columns directly instead of routing through an unrelated capture callback. |
| !7282 | merged | medium | Packet recoloring is invoked directly on PacketList instead of through a global UI wrapper plus a second reset call. |
| !7281 | merged | low | Removes packet-list prototypes for functions that have no implementations. |
| !7280 | merged | high | `setSortingEnabled()` is called only when the desired state differs because toggling it can resort/rebuild the list and, when repeated, contribute to invalid view-model state and crashes. Avoid side-effectful “set” operations when no state transition is required. |
| !7279 | merged | medium | Perl→Python tooling port preserves output semantics while giving the converter a normal Python CLI. Discussion also confirms the old tool had no current in-tree caller, so behavior compatibility matters more than preserving invocation accidents. |
| !7278 | merged | low | Release-branch backport: restores the checkable property on “Resolve Names” actions. |
| !7277 | merged | low | Release-branch backport of the same Resolve Names UI fix. |
| !7276 | merged | medium | Master fix: an action whose checked state is meaningful must actually be configured as checkable. |
| !7275 | closed | review-only | Guy Harris proposes validating token count and minimum token lengths before indexing/slicing EAP identity strings. He also flags a deeper unresolved issue: identity data may not be correctly modeled as ASCII. Useful defensive-parsing review, but not accepted implementation precedent in this MR. |
| !7274 | merged | low | Version metadata moves from 3.7.1 to 3.7.2 across CMake, docs, and Debian packaging. |
| !7273 | merged | medium | WSDG Lua docs are corrected to use the actual `FieldInfo.range` API. Contributor notes also document the then-current GitLab workflow: leave rebasing/merge handling to the project utility after review rather than pressing Rebase manually. |
| !7272 | merged | low | 3.7.1 release-build bookkeeping and release-note finalization. |
| !7271 | closed | review-only | Duplicate attempt at the `dfvm.c` uninitialized-reference warning. Roland Knall points to !7267 as the existing fix. |
| !7270 | merged | low | Speeds progress-frame appearance/animation to reduce rendering overhead during loading/dissection and make short operations visible. |
| !7269 | merged | high | STUN heuristic recognition rejects method/channel ranges reserved by RFC 5764/7983 for multiplexing, while preserving the explicit Microsoft exception. Standards-defined impossible/reserved values are valuable negative heuristic evidence. |
| !7268 | merged | medium | DoIP adds a UAT for user-defined payload-type names, with configured names taking precedence over static names and packet-scope formatted fallback text for display. |
| !7267 | merged | high | Accepted Coverity fix initializes `ref` and `layer` at the top of each loop iteration and rejects empty input before dereference. It repairs the control/data flow that caused the warning. |
| !7266 | closed | high negative | Gerald Combs initially proposed null-initializing `ref` and guarding the use. Roland Knall explains that this only masks the symptom; the value must be assigned before the branch that uses it. Gerald closes the MR in favor of the structural fix. |
| !7265 | merged | medium | STUN comments are updated to accurately document standards/version differences and reserved TURN channel ranges; protocol commentary is treated as part of maintainable decoder semantics. |
| !7264 | merged | medium | Python 3 scripts use `#!/usr/bin/env python3` and executable mode consistently. |
| !7263 | merged | low | Automated registry/generated-data refresh on master. |
| !7262 | merged | low | Automated registry/generated-data refresh on a release line. |
| !7261 | merged | low | Automated registry/generated-data refresh on another release line. |

## Highest-value durable conclusions

### UAT reset is part of derived-state correctness

Merged !7310 does more than add validation messages. SOME/IP’s UATs maintain secondary hash tables and dynamically registered state derived from accepted records. The accepted change splits “destroy old derived state” into reset callbacks and wires those callbacks into UAT registration, so reload/reset/error paths cannot continue using stale tables built from a previous configuration. It also rejects direct self-reference in recursive array/struct/union definitions and puts the offending record ID into validation errors.

**Durable rule:** if a UAT owns caches, indexes, hash tables, or dynamically registered state derived from its rows, implement the UAT reset path so that state is torn down independently of successful post-update reconstruction. Validation failure or table reset must not leave old derived state live.

### Fix analyzer findings at the data-flow cause, not with a guard that changes semantics

Closed !7266 and merged !7267 form a useful negative/positive pair. The warning came from using `ref` before it was assigned in the `range == NULL` branch. Guarding the use with `ref != NULL` suppresses the bad access but also changes the intended behavior by skipping values. The accepted correction moves acquisition of the current element and its layer before either branch and separately rejects an empty input container.

**Durable rule:** for an uninitialized-value report, identify where the semantic value should become defined and make that data flow explicit. Do not add a null/default guard merely to silence the analyzer when the operation is required for correct behavior.

### Separate heuristic recognition from explicit dispatch

Merged !7305 splits DCP-ETSI into a strict heuristic wrapper and an ordinary dissector registered for Decode As. Merged !7302 and !7269 independently tighten content recognition before claiming traffic, while !7292 shows that once a valid packet establishes the protocol, conversation state can safely route later weakly distinguishable message forms through the non-heuristic dissector.

**Durable rule:** heuristic entry points should answer “is this mine?” using protocol evidence; ordinary dissectors should perform the actual dissection and remain available to explicit dispatch such as Decode As. After strong recognition, conversation state may provide subsequent dispatch without weakening the heuristic for unrelated traffic.

### Default port bindings should track authoritative assignments

Merged !7303 removes automatic KNX/IP claims on commonly used but unregistered ports 3672 and 40000 and defaults the range preference only to IANA-assigned 3671.

**Durable rule:** do not turn deployment convention into a global automatic binding when the codepoint is not authoritatively assigned to the protocol. Prefer a user-configurable range / Decode As path for additional deployment ports.

### Standard checksum presentation is part of the API contract

Merged !7304 computes STUN’s protocol-specific fingerprint value but passes the verification result through `proto_tree_add_checksum()`, yielding the standard value/status/expert presentation.

**Durable rule:** keep protocol-specific checksum math where needed, but use Wireshark’s checksum helper for tree/status/expert representation whenever its model fits.

### Expensive model access belongs outside comparator repetition

Merged !7296 reduces packet-list sort work by evaluating `columnString()` once per side and reusing the resulting QStrings for all comparisons and optional numeric parsing. The comparator executes at scale, so apparently small repeated model/materialization work becomes significant.

**Durable rule:** in hot comparison/sort paths, materialize expensive derived values once per comparison and reuse them; do not repeatedly cross model/formatting boundaries for identical operands.
