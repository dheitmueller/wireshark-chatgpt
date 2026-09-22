# Dissector Registration Conventions

This file records durable guidance about automatic dissector registration and default protocol bindings.

## Do not claim an obsolete or unregistered port without enough discrimination to avoid false positives

A historical implementation using a port number is not, by itself, sufficient reason for Wireshark to claim that port automatically forever. Automatic registration should reflect a current protocol assignment or have enough additional discrimination to make the binding reliable.

Merged MR !23056, authored by John Thacker, removes the default UDP port 921 registration from the lwres dissector. lwresd had been removed from BIND years earlier, port 921 was never assigned by IANA for lwres, and the dissector did not constrain the match by loopback address or any other heuristic. Keeping the automatic binding therefore risked interpreting unrelated traffic on an unregistered port as lwres.

**Registration rule:** before adding or retaining a default port binding, verify that the port is actually assigned/authoritative for the protocol or that the dissector has another reliable discriminator. For obsolete protocols using historically conventional but unregistered ports, prefer explicit Decode As / user configuration over globally claiming the port when unrelated traffic could plausibly use it.

**Confidence:** Very high. Merged master change authored by John Thacker and approved/merged by Anders Broman.

## Do not claim shared local or experimental protocol codepoints in public builds

A protocol that temporarily uses a standards-defined local or experimental discriminator does not own that value globally. Registering a public Wireshark dissector directly on such a value makes every packet using the shared experimental value look like that one protocol and can conflict with unrelated experiments.

Merged MR !22847 initially registered the new ESUN dissector on IEEE Local Experimental EtherType `0x88B5`. John Thacker explicitly rejected that for public distribution and requested `dissector_add_for_decode_as()` instead, so interested users could select ESUN with Decode As and optionally save the mapping in a profile. He further noted that direct registration can be added after a final IEEE EtherType assignment exists. The contributor made exactly that change, the discussion was resolved, and John later merged the MR.

**Registration rule:** if the protocol has no uniquely assigned discriminator and is using a local/experimental value that other protocols may legitimately reuse, expose the dissector through Decode As (or another explicit user-selection mechanism) rather than claiming the shared value automatically. Add fixed registration only when the protocol receives an authoritative assignment or another discriminator makes automatic recognition unambiguous.

**Confidence:** Very high. Direct John Thacker review on a merged master MR, with the requested registration model implemented before John merged it.

## Keep table-driven registration predicates on the same indexed entry

When iterating a registration table, every predicate controlling whether an operation is registered must inspect the same `table[i]` element whose discriminator and handler are then registered. Accidentally testing `table->member` repeatedly while registering `table[i]` can suppress later entries or make branches unexpectedly unreachable.

Merged master MR !13202, authored and merged by John Thacker, fixes decade-old ISDN supplementary-service handoff code after Clang 17 diagnosed unreachable code. The loop registered `isdn_sup_global_op_tab[i]` but tested `isdn_sup_global_op_tab->arg_pdu` and `isdn_sup_global_op_tab->res_pdu`, i.e. element zero, on every iteration. The accepted fix indexes those predicates with `[i]` as well.

**Implementation rule:** in table-driven handoff and registration loops, keep the condition, discriminator/key, and callback/handle derived from the same indexed record. A local pointer to the current entry can make accidental cross-entry access harder to write.

**Review rule:** treat compiler or static-analyzer "unreachable code" diagnostics in long-lived registration loops as possible evidence that the controlling predicate refers to the wrong table element, rather than dismissing them as warning noise.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with Clang 17 exposing a decade-old semantic bug.
