# Wireshark Protocol-Layer State Ownership Conventions

## Split mutable state at independent protocol-layer reset boundaries

A convenient combined structure is unsafe when separate protocol layers populate or clear different parts of it independently. A reset in one parser subtree can silently erase state learned by another layer even when both structures share identifiers such as UE or bearer IDs.

Merged master MR !1556 initially used one NR DRB mapping structure for both MAC/RLC and RLC/PDCP configuration. Pascal Quantin pointed out that entering `RLC-BearerConfig` cleared that structure, which could destroy independently collected PDCP state. Martin Mathieson agreed that the object was spanning two different layer ownership domains and split it into separate `nr_drb_mac_rlc_mapping_t` and `nr_drb_rlc_pdcp_mapping_t` structures before merge.

**Architecture rule:** group mutable dissector state by the semantic component that owns its initialization and reset lifecycle. Shared identifiers do not justify shared storage when independent protocol subtrees can reset or repopulate the data at different times.

**Review rule:** for a state structure populated from multiple protocol layers or ASN.1 subtrees, trace every initialization or memset path. If one subtree can clear fields owned by another, split the state or give each owner an independent lifetime.

**Confidence:** Very high. Merged master change with the failure mode identified explicitly by Pascal Quantin and the accepted redesign performed by Martin Mathieson.
