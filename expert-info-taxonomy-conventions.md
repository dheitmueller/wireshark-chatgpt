# Wireshark Expert-Info Taxonomy Conventions

This file records durable conventions for choosing expert-info groups and keeping those categories available consistently across Wireshark APIs. Current upstream source remains authoritative.

## Classify diagnostics by the layer that originated the condition

Expert information is not only a severity level; its group should describe the kind of condition being reported. Capture-receive failures and interface/device events are semantically different from malformed protocol data, even when both ultimately produce a warning or error in the packet view.

Merged master MR !14255 was authored and merged by Guy Harris and adds two explicit expert groups. `PI_RECEIVE` is for indications associated with receiving packets, such as CRC errors and short/long-frame indications. `PI_INTERFACE` is for interface indications not tied to packet reception itself, such as out-of-buffer conditions, hardware errors, and link-speed changes. The accepted change also exposes the new groups through WSLua and updates the surrounding documentation comments rather than adding C-only enum values that scripting users cannot select.

**Architecture rule:** choose an expert group from the semantic source of the condition, not merely from the protocol currently owning the tree item. Packet-receive status belongs in a receive-oriented group; device/interface status belongs in an interface-oriented group; protocol-structure failures remain in protocol/malformed-style groups as appropriate. Severity (`PI_NOTE`, `PI_WARN`, `PI_ERROR`, etc.) is a separate axis.

**API rule:** when a public expert taxonomy is extended, carry the new category through all supported front ends and bindings that expose that taxonomy, including WSLua and developer documentation where applicable. Do not leave scripting/plugin APIs with a smaller or stale category set unless that limitation is intentional and documented.

**Confidence:** Extremely high. The merged change was authored, iterated, approved, and merged by Guy Harris, and its descriptions explicitly define the intended semantic boundary between receive and interface indications.

## Expert-info fields are presence markers, not Boolean-valued protocol fields

Expert-info generated fields have `FT_NONE` semantics: they represent the presence of a diagnostic, not a stored true/false value. Code consuming those fields should therefore test whether an instance exists rather than attempting to extract a Boolean value from them.

Merged master MR !8473, authored by Guy Harris, fixes the Transum plugin's handling of `tcp.analysis.retransmission` and `tcp.analysis.keep_alive`. Both are expert-info fields. The accepted implementation counts field instances and treats presence as the condition instead of reading them through a Boolean extractor. Stable backports !8474 and !8475 carry the same semantic fix to maintained branches.

**Implementation rule:** when consuming an expert-info-generated field programmatically, inspect field presence/instance count. Do not infer a Boolean value from an `FT_NONE` expert field simply because its user-visible meaning sounds Boolean.

**Review rule:** distinguish a protocol Boolean (`FT_BOOLEAN`) from a diagnostic marker whose only state is present or absent. This matters for plugins, taps, exporters, and any code that reads protocol-tree fields directly.

**Confidence:** Extremely high. Merged master change authored by Guy Harris, with matching stable-branch backports.

### Historical precursor: type checking exposed the misuse

Merged master MR !8412, authored by Guy Harris, correctly updates TRANSUM's extractor for genuine `FT_BOOLEAN` values to use Wireshark's 64-bit Boolean storage and adds an assertion that the supplied field really is `FT_BOOLEAN`. Chuck Craft then reported that the new assertion fired for `tcp.analysis.retransmission`. Guy identified the deeper caller bug: the TCP analysis fields in question are expert-info fields, so they do not carry Boolean values at all. Already-reviewed !8473 is the authoritative final correction and switches TRANSUM to presence testing.

**Review implication:** a new type assertion can reveal a pre-existing caller/API misuse rather than a problem with the assertion. When a stricter extractor fails, verify the registered field type and semantic contract before weakening the check.

**Confidence:** Extremely high. The diagnostic and interpretation come directly from Guy Harris; the final behavior is confirmed by merged !8473 and its backports.



## Treat missing reassembly as a reassembly condition, not malformed protocol data

Merged master MR !6623, authored and merged by John Thacker, reports `FragmentBoundsError` with a `PI_REASSEMBLE` / `PI_NOTE` expert item suggesting that reassembly preferences may need to be enabled. Companion merged MR !6621 marks a partial TCP PDU TVB as fragmented when desegmentation is disabled or unavailable, so bounds handling reaches the unreassembled-fragment path instead of a malformed-packet error.

**Diagnostic rule:** incomplete data caused by disabled or unavailable reassembly belongs in reassembly-oriented diagnostics. Reserve malformed/error categories for structurally invalid input or genuine dissector/reassembly failures.

**Confidence:** Very high. Both merged core changes were authored by John Thacker and intentionally pair TVB fragment semantics with the expert-info taxonomy.

## Sequence loss is not malformed packet syntax

Merged stable MR !4474 changes MPEG-TS continuity-counter loss from `PI_MALFORMED` to `PI_SEQUENCE`. Individual packets can remain structurally valid while the observed stream has skipped sequence values.

**Diagnostic rule:** classify missing, duplicated, or out-of-order progression as a sequence condition when packet syntax is otherwise valid. Reserve `PI_MALFORMED` for invalid protocol structure; choose severity independently.

**Confidence:** High. Merged maintained-branch correction consistent with the taxonomy above.


## Validate group and severity independently

Merged master MR !2295 adds registration-time checks after finding expert-info declarations where group and severity values were swapped or drawn from the wrong set. The change is useful evidence that these are separate semantic domains even when represented by similar integer constants.

**Registration rule:** validate expert-info group and severity independently. Prefer APIs and naming that make the two domains difficult to interchange.

Merged !2273 adds a presentation caveat: a normal preference-disabled state that applies broadly need not become expert info if that would colorize or populate the expert view for otherwise ordinary packets.

**Confidence:** High. Merged changes with direct maintainer discussion.
