# Wireshark Wiretap Record/Interface Conventions

This file records durable Wiretap conventions for the relationship between capture records and interface metadata. Current upstream source and capture-format specifications remain authoritative.

## A record's encapsulation must agree with the interface that owns it

When a Wiretap reader creates Interface Description Blocks, packet records must be associated with an interface whose declared link type matches the record's encapsulation. A convenient existing interface is not interchangeable merely because the source capture format grouped the records together.

Merged master MR !12647 (`blf: Use WTAP_ENCAP_NONE, record linktypes must match interface linktype`), authored and merged by John Thacker, fixes BLF handling around this invariant. BLF discovers and creates its own interfaces while reading, so the initial file encapsulation is changed to `WTAP_ENCAP_NONE`, representing that no interface encapsulation has yet been established, rather than `WTAP_ENCAP_UNKNOWN`, which represents unknown/bogus encapsulation. The accepted change also stops emitting upper-PDU status records through an Ethernet interface: those records require an interface whose link type matches their own encapsulation.

**Implementation rule:** if a reader synthesizes IDBs, establish or select an interface whose link type agrees with each record before publishing the record/interface association. Do not reuse an interface across records with incompatible encapsulations, even when they share a source channel or other format-local identity.

**State rule:** distinguish “no interface/link type established yet” from “the link type exists but is unknown or invalid.” Use the Wiretap sentinel whose semantics match the reader's lifecycle rather than treating sentinel values as interchangeable initialization defaults.

**Testing rule:** include captures where one source container produces records of different Wiretap encapsulations and verify that each record resolves through an IDB with a matching link type after read/save/export paths.

**Confidence:** Very high. Merged master Wiretap correctness fix authored and merged by John Thacker, with both the sentinel distinction and record/IDB link-type invariant stated directly in the change.
