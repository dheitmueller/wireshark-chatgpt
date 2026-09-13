# Dissector pipeline and side-effect conventions

## Separate recursive semantic parsing from one-time packet side effects

A dissector that can recursively parse nested/decrypted containers should keep the recursive parsing operation distinct from packet-level side effects such as final column text and tap queueing. Nested parsing can accumulate packet-scoped state, while the top-level entry point performs the one-time presentation/tap actions after all nested content has contributed.

Do not use a NULL protocol tree as a shortcut for skipping semantic processing when that processing affects columns, taps, decryption/signature results, reassembly, or other non-tree state. The protocol-tree APIs already handle NULL trees efficiently; an additional parser shortcut is only valid when it is semantically equivalent.

**Evidence:** merged master MR !20690, authored and merged by John Thacker, restructures collectd so recursive part parsing accumulates column and tap information in packet-scoped protocol data, while the outer dissector sets columns and queues tap data once. It removes a special NULL-tree shortcut because it was already incorrect for signed/encrypted packets and would become still less valid when decryption and signature checking affect summary state.

## Keep packet context valid even when optional presentation is disabled

If a helper sometimes suppresses expert information or column updates, prefer an explicit behavior flag over abusing a NULL `packet_info *` as that signal. Packet context has other responsibilities, including allocator lifetime and access to per-packet state; conflating "do not emit these optional side effects" with "there is no packet context" makes later API cleanup harder and can force reliance on ambient globals/scopes.

**Evidence:** merged master MR !20714 replaces NULL `packet_info *` signaling in the WPS helper with an explicit flag, specifically so `pinfo` remains available and ambient `wmem_packet_scope()` use can be replaced. This corroborates the notebook's existing explicit packet-context/allocator-scope guidance.

## Parser-engine replacements should preserve observable registration semantics

When replacing the implementation used to parse configuration/schema input, separate the parser-engine substitution from changes to protocol registration timing, field registration behavior, and other observable semantics unless those semantic changes are independently intended and validated. Compare old and new generated/registered output where practical.

**Evidence:** merged master MR !20684 replaces the XML DTD Flex parser with libxml2 while deliberately restoring the established field-registration flow rather than carrying forward broader behavior changes from an earlier attempt. Michael Mann supplied old/new field-output comparisons, and John Thacker explicitly reviewed the revised approach as the right direction while checking remaining DTD semantic differences. Closed draft !20718 explored a broader version and is therefore treated as superseded context rather than accepted precedent.
