# Wireshark MR automation review: !21741-!21790

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to oldest
MRs reviewed in this run: 50

Selection was built from the union of all existing `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md`, rather than assuming numeric ranges were complete. The historical !17571-!17620 batch remains part of the already-reviewed set. No previously reviewed sparse exception occurred inside this batch, so the fifty highest-numbered unreviewed corpus entries were exactly !21790 through !21741.

## Exact reviewed MR set

- !21790
- !21789
- !21788
- !21787
- !21786
- !21785
- !21784
- !21783
- !21782
- !21781
- !21780
- !21779
- !21778
- !21777
- !21776
- !21775
- !21774
- !21773
- !21772
- !21771
- !21770
- !21769
- !21768
- !21767
- !21766
- !21765
- !21764
- !21763
- !21762
- !21761
- !21760
- !21759
- !21758
- !21757
- !21756
- !21755
- !21754
- !21753
- !21752
- !21751
- !21750
- !21749
- !21748
- !21747
- !21746
- !21745
- !21744
- !21743
- !21742
- !21741

## Durable finding promoted

- **!21759 — Deep / merged master / very high weight.** John Thacker explains a nested WSLua crash mechanism caused by sharing one Lua `errorJmp` chain across nested dissector calls while Wireshark's independent `setjmp`/`longjmp` exception mechanism can unwind an inner Lua protected call. The merged fix gives each call a `lua_newthread()` execution context so globals remain shared but transient stack/jump-buffer state does not. John exercised several historical Lua crashers, large captures, and plugin reloads; Stig Bjørlykke independently confirmed the result. Added the nested-call variant to `exception-boundary-conventions.md`.

## Strong corroborating evidence

- **!21790 — Deep / merged master.** Michael Mann moves NCSI-specific generated PCI data out of generic `epan` and into the dissector directory. This directly corroborates the existing rule that protocol-specific generated code/data belongs with dissectors rather than in the generic dissection engine.
- **!21789 — Discussion-focused / closed, down-weighted implementation evidence.** Michael Mann, Alexis La Goutte, and Jaap Keuter reject/rescope a contribution sourced from the contributor's `master` branch and require recreation from a proper topic branch. Strong submission-process corroboration, but not an accepted implementation exemplar.
- **!21777 — Deep / merged master.** John Thacker fixes TFTP conversation lookup/session reuse. It strongly corroborates the convention already extracted from !21803: transport endpoint tuples can outlive a protocol transaction, so per-transfer state must be renewed or separately keyed when ports are reused.
- **!21776 — Deep / merged master.** Alexis La Goutte explicitly asks a first-time contributor for a pcap for a new DNS AMTRELAY dissector. The contributor supplies a synthetic capture generated from a hacked library and validates its bytes against the RFC. This independently corroborates the existing sample-capture expectation, including that a controlled synthetic capture is useful when real traffic is unavailable.
- **!21780 — Deep / merged master.** Michael Mann abstracts UUID/GUID/RPC lookup data behind a generic interface so UI code does not directly call protocol-specific globals. Corroborates the existing application-layer boundary and registration/facade conventions.
- **!21784 — Scanned / merged master.** Warning cleanup driven by `tools/check_dissector.py`; corroborates the established pre-submit static-check workflow.

## Per-MR audit notes

- !21790 — merged; NCSI generated data moved into the dissector directory; architecture corroboration noted above.
- !21789 — closed/unmerged; SWm change submitted from `master`; maintainer discussion requires a clean non-master topic branch. Down-weighted as implementation evidence.
- !21788 — merged John Thacker PIDL cleanup; alignment metadata requiring numeric `0` rather than Perl `NULL`; generator-input correctness, no new general rule.
- !21787 — merged CQL error-dissection expansion; a later Alexis suggestion to use `switch`/`case` is style-level and post-merge, so no durable convention promoted.
- !21786 — merged radiotap tree-parent correction for unknown TLV/non-TLV items; local presentation correctness.
- !21785 — merged RTP conversation-debug variable rename; maintenance-only.
- !21784 — merged PROCMON `check_dissector.py` warning cleanup; corroborates existing static-analysis guidance.
- !21783 — merged large nftables/netlink-netfilter expansion with explicit AI-use notice; little substantive human review in the corpus snapshot, so accepted implementation carries more weight than discussion but yields no new convention.
- !21782 — Darwin BK_SYS typo correction/backport; straightforward protocol-value correctness.
- !21781 — merged LDA NEO trailer CRC/device-ID mask correction; protocol-specific bitmask fix.
- !21780 — merged generic UUID lookup interface; application/UI boundary corroboration noted above.
- !21779 — merged E2AP const-ification of immutable dissector metadata; useful cleanup, no new rule.
- !21778 — merged PIDL generated-filter-name correction (`$e->{NAME}` rather than an unavailable `$_->{NAME}`); generator correctness, no distinct convention.
- !21777 — merged TFTP conversation/session fix; strong corroboration of existing logical-session state rule.
- !21776 — merged DNS AMTRELAY dissector; maintainer-requested pcap supplied; testing corroboration noted above.
- !21775 — merged PIDL undefined-SwitchType ordering fix; defensive generator diagnostics, no new rule.
- !21774 — merged typo/user-visible-text cleanup; maintenance-only.
- !21773 — closed predecessor for DNS AMTRELAY; superseded by merged !21776 and therefore down-weighted.
- !21772 — Darwin BK_SYS correction in another branch; backport/release corroboration only.
- !21771 — merged GitLab CI merge-train optimization; CI scheduling-specific, no durable Wireshark coding rule promoted.
- !21770 — merged wiretap JSON-log extension harmonization; file-type recognition maintenance.
- !21769 — DNS HIP public-key algorithm field-name correction backport; no additional lesson.
- !21768 — DNS HIP public-key algorithm field-name correction backport; no additional lesson.
- !21767 — merged CMake policy requiring Qt 6 on Windows/macOS; build-policy transition, no new general convention.
- !21766 — merged Qt 5 deprecation warning/docs; build-policy transition.
- !21765 — version bump 4.2.14 to 4.2.15; release maintenance.
- !21764 — version bump 4.4.10 to 4.4.11; release maintenance.
- !21763 — version bump 4.6.0 to 4.6.1; release maintenance.
- !21762 — closed spelling-cleanup submission; low-value/unmerged evidence.
- !21761 — merged master DNS HIP public-key algorithm field-name correction; semantic filter-field naming correctness, already covered by existing field-semantics guidance.
- !21760 — merged PIDL warning behavior for unsupported `switch_is` discriminants; generator diagnostics, no new general rule.
- !21759 — merged WSLua nested exception-context fix; promoted to notebook as the principal new durable finding.
- !21758 — WSLua `TreeItem_set_len` illegal-length validation sibling/backport; range validation corroboration only.
- !21757 — WSLua `TreeItem_set_len` illegal-length validation sibling/backport; range validation corroboration only.
- !21756 — WSLua `TreeItem_set_len` illegal-length validation sibling/backport; range validation corroboration only.
- !21755 — WSLua `TreeItem_set_len` illegal-length validation sibling/backport; range validation corroboration only.
- !21754 — merged const-ification of CSNDESCR metadata; immutable-data cleanup.
- !21753 — merged SVCCTL generated parameter typo fix; local generated-code correctness.
- !21752 — merged PIDL `has_property` use to avoid comparing `undef` with an empty string; generator robustness and warning cleanup.
- !21751 — merged SAMR `hf_` rename fixes prompted by unused warnings; generated/field-name maintenance.
- !21750 — merged Qt graph-save default changed to PNG; UI preference behavior, no broad convention.
- !21749 — merged John Thacker DFS/PIDL bitmap fix; replaces enum-symbol references that PIDL misparsed as hex with supported literal values. Useful generator limitation evidence, but too tool-specific for a new general rule.
- !21748 — merged PIDL false-positive unused-parameter warning fix; generator diagnostics.
- !21747 — DNS HIP HIT algorithm field-name release backport; no additional lesson.
- !21746 — DNS HIP HIT algorithm field-name release backport; no additional lesson.
- !21745 — merged master DNS HIP HIT algorithm field-name correction; filter-field semantic naming maintenance.
- !21744 — merged NR RRC display/prettification change; protocol-specific presentation.
- !21743 — 4.2.14 release build metadata; release maintenance.
- !21742 — 4.4.10 release build metadata; release maintenance.
- !21741 — 4.6.0 release build metadata; release maintenance.

## Weighting notes

Merged master changes were treated as the strongest implementation evidence. Release backports and repetitive version/build changes were scanned for independent review information but not allowed to multiply the weight of the same underlying lesson. Closed !21789, !21773, and !21762 were explicitly down-weighted; maintainer comments in !21789 remain useful submission-policy evidence because they are direct statements from core reviewers. No MR in this run contained Guy Harris feedback that materially changed the extracted rules, so no artificial Guy-weight was assigned.
