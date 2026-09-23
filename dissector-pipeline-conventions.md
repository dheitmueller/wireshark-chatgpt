# Dissector pipeline and side-effect conventions

## Separate recursive semantic parsing from one-time packet side effects

A dissector that can recursively parse nested/decrypted containers should keep the recursive parsing operation distinct from packet-level side effects such as final column text and tap queueing. Nested parsing can accumulate packet-scoped state, while the top-level entry point performs the one-time presentation/tap actions after all nested content has contributed.

Do not use a NULL protocol tree as a shortcut for skipping semantic processing when that processing affects columns, taps, decryption/signature results, reassembly, or other non-tree state. The protocol-tree APIs already handle NULL trees efficiently; an additional parser shortcut is only valid when it is semantically equivalent.

**Evidence:** merged master MR !20690, authored and merged by John Thacker, restructures collectd so recursive part parsing accumulates column and tap information in packet-scoped protocol data, while the outer dissector sets columns and queues tap data once. It removes a special NULL-tree shortcut because it was already incorrect for signed/encrypted packets and would become still less valid when decryption and signature checking affect summary state.

## Keep packet context valid even when optional presentation is disabled

If a helper sometimes suppresses expert information or column updates, prefer an explicit behavior flag over abusing a NULL `packet_info *` as that signal. Packet context has other responsibilities, including allocator lifetime and access to per-packet state; conflating "do not emit these optional side effects" with "there is no packet context" makes later API cleanup harder and can force reliance on ambient globals/scopes.

**Evidence:** merged master MR !20714 replaces NULL `packet_info *` signaling in the WPS helper with an explicit flag, specifically so `pinfo` remains available and ambient `wmem_packet_scope()` use can be replaced. Merged master MR !12332 independently fixes the same design error in AFP: `decode_name_label()` had used NULL `pinfo` to mean "do not update the Info column", but the helper also needed `pinfo->pool` for string lifetime. The accepted fix passes valid packet context on every call and adds a separate `add_info` boolean for the optional column side effect. Together these changes reinforce that behavior policy and required packet context are separate API concerns.

## Parser-engine replacements should preserve observable registration semantics

When replacing the implementation used to parse configuration/schema input, separate the parser-engine substitution from changes to protocol registration timing, field registration behavior, and other observable semantics unless those semantic changes are independently intended and validated. Compare old and new generated/registered output where practical.

**Evidence:** merged master MR !20684 replaces the XML DTD Flex parser with libxml2 while deliberately restoring the established field-registration flow rather than carrying forward broader behavior changes from an earlier attempt. Michael Mann supplied old/new field-output comparisons, and John Thacker explicitly reviewed the revised approach as the right direction while checking remaining DTD semantic differences. Closed draft !20718 explored a broader version and is therefore treated as superseded context rather than accepted precedent.

## Internalize mandatory dissection preparation in the owning library layer

When a dissection prerequisite can be derived from libwireshark's own state, perform it at the common library entry point rather than requiring every frontend or caller to remember an extra preparatory call. This keeps first-pass behavior consistent across Wireshark, TShark, and other consumers and prevents new frontends from silently omitting a required setup step.

**Evidence:** merged master MR !20384, authored by John Thacker and approved/merged by Anders Broman, moves postdissector field priming into the common record/file dissection path. The library already knows whether the frame has been visited and whether postdissectors want fields on the first pass, so callers no longer need to remember to invoke the priming API themselves. The stated rationale is to centralize the logic and make subsequent changes easier.

**Implementation rule:** if preparation is an invariant of the dissection pipeline rather than application policy, make the pipeline enforce it. Expose separate setup APIs only when callers genuinely need to choose whether or how the preparation occurs.

## Update semantic state before rendering fields that depend on it

When a packet both establishes state and displays generated fields derived from that state, perform the state update before the presentation step. Otherwise the packet that first creates the relationship can be the one packet that does not display it, with only later packets benefiting from the newly learned state.

**Evidence:** merged master MR !20302 moves DIAMETER, GTP, GTPv2, and PFCP session tracking ahead of adding associated-session information to the protocol tree. The stated result is that the associated IMSI is available on the first request itself rather than only after that request has already been rendered. Review discussion considered duplicate presentation of an IMSI already present elsewhere in the packet, and the merged implementation favored consistent generated association information after state tracking is complete.

**Implementation rule:** structure a dissection pass as semantic acquisition/update followed by presentation of derived state when the current packet is itself capable of changing that state. Do not make generated fields accidentally one packet late merely because tree construction precedes bookkeeping in source order.

**Confidence:** High. Merged master behavior correction across four related protocol dissectors, with explicit review of the resulting generated-field presentation.