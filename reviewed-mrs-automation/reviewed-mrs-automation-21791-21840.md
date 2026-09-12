# Wireshark MR automation review: !21791-!21840

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to oldest
MRs reviewed in this run: 50

Selection was built from the union of all existing `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md`, rather than assuming numeric ranges were complete. The historical !17571-!17620 batch remains part of the already-reviewed set. No previously reviewed sparse exception occurred inside this batch, so the fifty highest-numbered unreviewed corpus entries were exactly !21840 through !21791.

## Exact reviewed MR set

- !21840
- !21839
- !21838
- !21837
- !21836
- !21835
- !21834
- !21833
- !21832
- !21831
- !21830
- !21829
- !21828
- !21827
- !21826
- !21825
- !21824
- !21823
- !21822
- !21821
- !21820
- !21819
- !21818
- !21817
- !21816
- !21815
- !21814
- !21813
- !21812
- !21811
- !21810
- !21809
- !21808
- !21807
- !21806
- !21805
- !21804
- !21803
- !21802
- !21801
- !21800
- !21799
- !21798
- !21797
- !21796
- !21795
- !21794
- !21793
- !21792
- !21791

## Durable findings promoted

- !21805, with release-4.6 backport !21807 — merged master lifetime fix authored and merged by John Thacker. `pinfo->conv_elements` retained addresses from BPv7 protocol-private objects whose lifetime could end earlier; the accepted code uses `pinfo->src` / `pinfo->dst`, already copied into `pinfo->pool`. Packet-owned structures must only retain references with packet-compatible lifetime. Promoted to `memory-lifetime-conventions.md`.
- !21803 — merged release-4.6 correctness fix authored by John Thacker. TFTP port/address tuples can be reused for multiple transfers, while `tftp_conv_info_t` represents one transfer. The fix creates a fresh conversation at each protocol-defined RRQ/WRQ start rather than silently reusing stale single-transfer state. More generally, conversation state lifetime must match the logical session; a broader endpoint conversation is only appropriate if it contains separately keyed per-session state. Promoted to `dissector-state-conventions.md`.
- !21811 — merged master ASN.1 generator change by Stig Bjørlykke, with explicit post-merge compatibility feedback from John Thacker. Enabling constraints by default is appropriate, but an existing `-C` enable option should not silently be inverted to mean disable; preserve old option semantics during migration, add an explicit inverse if needed, or remove the transitional option once migration is complete. Promoted to `generated-code-conventions.md`.

## Strong corroborating evidence retained without duplicate notebook rules

- !21830 — merged master OSS-Fuzz fix authored by John Thacker. Delay decoding the length until the non-terminator form establishes that the length field exists, use an unsigned semantic type, and let TVBuff/proto-tree APIs perform bounds checking before setting the final item length. Reinforces existing checked parser-arithmetic and semantic-domain guidance.
- !21801 — merged master PIDL generator fix authored by John Thacker. A `MANUAL` override means the replaced generated function should not be calculated at all; otherwise the generator can emit misleading unsupported-feature diagnostics for code that will never be used. Reinforces generated-source-of-truth and generator-aware override handling.
- !21822, !21821, !21820, !21819, !21812, and !21809 — merged BER/generated-dissector cleanup around `hf_id == -1`. A caller that intentionally requests value extraction without a tree item should not get a synthetic/unknown tree item as a side effect; reinforces semantic API contracts for generated dissectors.
- !21831 and !21810 — merged SGP22 type-export improvements continue the prior batch's move toward generator-derived export/type metadata rather than manual parallel configuration.
- !21833 and !21813 — merged generated-dissector maintenance fixes reinforce that conformance/manual metadata, generated names, enum/type indirections, and emitted code must remain synchronized when source definitions evolve.
- !21799, !21792, and !21791 — merged master architecture cleanup moves protocol-specific declarations/constants out of generic `epan/` headers and into protocol/dissector-owned headers. Strongly corroborates the existing rule that `epan` is the generic dissection engine, not a repository for protocol-specific shared knowledge.
- !21795 — merged master Michael Mann refactor, merged by John Thacker, moves generic application memory-usage functionality to `wsutil`; corroborates placing truly generic utility capabilities in the lowest common owning layer.
- !21800 — merged master build fix authored and merged by John Thacker. `ws_assert` disappears in Release builds, so code must not depend on an assertion as the only use of a variable; reinforces validating both debug and release configurations and treating assertions as checks rather than program structure.
- !21829 and the corresponding tshark/capture-sync cleanup/backports !21823, !21827, and !21828 — merged fixes from Guy Harris around exact return-value/error handling reinforce reading helper contracts literally and keeping stable-branch backports mechanically aligned with the accepted master fix.
- !21793 — merged radiotap tree-parent correction reinforces placing protocol items beneath the semantically correct subtree rather than the packet root merely because both are available.
- !21802 — merged TCP structure-layout optimization reduces padding without changing semantics; useful implementation evidence, but no broader notebook rule was needed.
- !21794 — merged 24-bit linear-audio codec support is a focused feature addition with no durable cross-cutting convention beyond existing codec/format validation practice.
- !21839 through !21834, !21818 through !21815, and !21798 — merged wmem Doxygen improvements clarify allocator/container APIs and ownership semantics but do not alter implementation contracts; retained as documentation-quality evidence rather than new architecture rules.
- !21840 and !21824-!21826 — automated data/translation updates were checked for scope and outcome but carry little reusable review guidance.

All other MRs in the exact set above were reviewed for discussion, target branch, outcome, and diff significance. Merged master changes were weighted most heavily; stable-branch cherry-picks/backports were treated primarily as corroboration of the corresponding accepted behavior. No abandoned or superseded MR in this batch supplied stronger implementation evidence than the merged changes above.