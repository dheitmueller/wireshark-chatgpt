# Wireshark Packet-Column Composition Conventions

This file records durable conventions for composing packet-list column text when one logical message can contribute one or several independently dissected items. Current upstream column APIs and dissector behavior remain authoritative.

## Design delimiters for both singleton and repeated contributions

Column fragments that can be emitted repeatedly within one packet must compose correctly when there is exactly one contribution and when there are several. Do not place opening/closing punctuation unconditionally inside every repeated element when that makes multi-element output malformed, and do not optimize the multi-element case at the expense of awkward singleton output.

Merged master MR !15334 adds callable NR-RRC SIB dissectors and adjusts `COL_INFO` presentation. Pascal Quantin explicitly rejected an intermediate display because it would not render cleanly when several SIBs occurred in one System Information message. The accepted revision handles the grouping/separator state so a single SIB and a list of multiple SIBs both produce coherent Info-column text. The change was authored by Anders Broman and merged after Pascal's protocol-focused review; stable !15337 corroborates the resulting behavior.

**Implementation rule:** before changing an Info-column fragment emitted from a repeated/nested dissector path, enumerate the zero-, one-, and many-element renderings. Give container-level punctuation to code that knows the aggregate boundary, or carry explicit first/last state when each element must participate in formatting.

**Review rule:** test column output with the smallest singleton case and with multiple adjacent contributions in the same packet. Column text is a composed user interface; a fragment that looks correct in isolation can still be wrong when another instance appends immediately after it.

**Confidence:** Very high. The cardinality/composition problem was identified directly by Pascal Quantin during review of a merged master NR-RRC change, and the implementation was revised before merge.