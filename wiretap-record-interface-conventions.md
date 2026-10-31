# Wireshark Wiretap Record/Interface Conventions

This file records durable Wiretap conventions for the relationship between capture records and interface metadata. Current upstream source and capture-format specifications remain authoritative.

## A record's encapsulation must agree with the interface that owns it

When a Wiretap reader creates Interface Description Blocks, packet records must be associated with an interface whose declared link type matches the record's encapsulation. A convenient existing interface is not interchangeable merely because the source capture format grouped the records together.

Merged master MR !12524 (`blf: Use WTAP_ENCAP_NONE, record linktypes must match interface linktype`), authored and merged by John Thacker, fixes BLF handling around this invariant. BLF discovers and creates its own interfaces while reading, so the initial file encapsulation is changed to `WTAP_ENCAP_NONE`, representing that no interface encapsulation has yet been established, rather than `WTAP_ENCAP_UNKNOWN`, which represents unknown/bogus encapsulation. The accepted change also stops emitting upper-PDU status records through an Ethernet interface: those records require an interface whose link type matches their own encapsulation. MR !12647 is the accepted release-4.2 cherry-pick of the same master change and therefore corroborates its stable-branch suitability rather than serving as the original master evidence.

**Implementation rule:** if a reader synthesizes IDBs, establish or select an interface whose link type agrees with each record before publishing the record/interface association. Do not reuse an interface across records with incompatible encapsulations, even when they share a source channel or other format-local identity.

**State rule:** distinguish “no interface/link type established yet” from “the link type exists but is unknown or invalid.” Use the Wiretap sentinel whose semantics match the reader's lifecycle rather than treating sentinel values as interchangeable initialization defaults.

**Testing rule:** include captures where one source container produces records of different Wiretap encapsulations and verify that each record resolves through an IDB with a matching link type after read/save/export paths.

**Confidence:** Very high. Merged master Wiretap correctness fix authored and merged by John Thacker, followed by an accepted release-4.2 backport, with both the sentinel distinction and record/IDB link-type invariant stated directly in the change.

## Preserve pcapng record-option ownership across dissection and later writing

Merged John Thacker master MR !9716 and stable backport !9727 fix TShark's single- and second-pass paths. `epan_dissect_run_with_taps()` and `epan_dissect_reset()` may unreference `rec->block`, but TShark still needs pcapng packet options to write the record later. The accepted code calls `wtap_block_ref(rec->block)` before dissection and restores the retained block reference after reset, leaving the usual record cleanup responsible for final release.

**Rule:** A processing call may change metadata reference ownership. Retain an explicit reference when a later output consumer needs the same block; test read/dissect/write option fidelity in both pass modes. **Confidence:** Very high, merged master plus backport.
