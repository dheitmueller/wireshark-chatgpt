# Automated MR review: !12913 through !12962

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook repository: `dheitmueller/wireshark-chatgpt`

Selection method: reconstructed the already-reviewed set from the available per-run ledgers under `reviewed-mrs-automation/` and `reviewed-mrs.md`, preserving the historical !17571-!17620 batch. The previous read of !12962 was only a frontier probe and was not treated as a completed review. The 50 highest-numbered corpus MRs not in the reviewed set were then selected individually; the resulting set happens to be contiguous.

## Exact reviewed MR set

- !12962
- !12961
- !12960
- !12959
- !12958
- !12957
- !12956
- !12955
- !12954
- !12953
- !12952
- !12951
- !12950
- !12949
- !12948
- !12947
- !12946
- !12945
- !12944
- !12943
- !12942
- !12941
- !12940
- !12939
- !12938
- !12937
- !12936
- !12935
- !12934
- !12933
- !12932
- !12931
- !12930
- !12929
- !12928
- !12927
- !12926
- !12925
- !12924
- !12923
- !12922
- !12921
- !12920
- !12919
- !12918
- !12917
- !12916
- !12915
- !12914
- !12913

Count: **50**.

## Durable findings promoted

- **!12960 — display-filter macro syntax.** The new `$name(...)` spelling is still recursive textual macro expansion rather than function evaluation. Review discussion exposed precedence surprises (notably negation of a macro body containing `or`) and correctly treated implicit parenthesization as a separate backward-incompatible semantic change. Added to `dfilter-conventions.md`.
- **!12941 — display-filter byte-reference slicing.** Semantic capabilities must follow the value represented by an AST node, not accidental node-kind coverage. `STTYPE_REFERENCE` to an `FT_BYTES` field is sliceable just like the ordinary field. The accepted change adds explicit semantic handling and a regression test. Added as corroboration/extension in `dfilter-conventions.md`.
- **!12934 — preference effect classification.** John Thacker's merged master fix restores a nonzero GUI-only preference effect while removing the default dissection effect, avoiding costly redissection for font/color/column/UI changes while preserving Lua's ability to detect that a preference changed. Submodules require an explicit effect audit because their defaults do not simply inherit the parent's classification. Added to `state-refresh-conventions.md`.
- **!12920 and release-4.2 backport !12923 — SDK versus runtime capability detection.** Guy Harris's master fix documents a macOS/Xcode 15 failure where the Sonoma SDK advertises libpcap symbols absent from older supported runtime libraries; a successful CMake link probe therefore did not prove deployment availability and the executable could fail before source-level availability guards ran. Added as `platform-capability-detection-conventions.md` with especially high evidentiary weight.

## Strong corroboration and review evidence retained without duplicate notebook rules

- **!12958 / !12962 — GTP/GTPv2 container migration.** Replacing file-lifetime `GHashTable` state with `wmem_map_t` allocated from `wmem_file_scope()` aligns the container with Wireshark's allocator lifecycle and avoids mixed GLib/wmem ownership. The master MR also documents differential validation with identical `tshark -2nVr` output. This strongly corroborates existing wmem/container-lifetime guidance.
- **!12957 — RF4CE illegal memory access fixes.** The accepted work hardens address handling for packets whose address representation is not the assumed EUI-64 form. Pascal Quantin also pushed repeated address-resolution logic down into the common helper rather than duplicating type-sensitive handling at each caller; the author refactored accordingly. Retained as supporting evidence for centralizing representation-sensitive validation at the shared abstraction boundary.
- **!12936 followed by !12951 / !12961 — QUIC connection-state assumptions.** The greased-QUIC-bit support introduced connection-state-dependent behavior; the subsequent null-check fix, identified by Coverity, demonstrates why optional conversation/connection state must be guarded on every path. This corroborates existing state/nullability guidance rather than creating a duplicate rule.
- **!12937 / !12947 / !12929 — TCP Segment Len presentation.** A calculated/generated field whose value is not present in packet bytes must not claim an offset/length span into the tvbuff. Retained as dissector-tree presentation evidence.
- **!12946 — Qt Preferences Apply.** The accepted change removes a workaround for an old Qt nested-event-loop bug after both the minimum Qt version and dialog lifecycle eliminated the trigger, and removes duplicate signal connections before adding Apply behavior. Useful evidence for retiring compatibility workarounds when their preconditions no longer exist.
- **!12922 — IP/CIDR consolidation.** A broad merged internal consolidation co-locates related IP/CIDR helpers and moves protocol-specific IPv6 definitions back to the dissector layer. Useful architectural evidence, but no stronger general rule than existing API/layer-boundary guidance.
- **!12955 — GitLab CI merge-request feedback plumbing.** Reviewed as CI infrastructure evolution. The discussion history is mostly implementation iteration/system notes; no additional durable rule was promoted beyond existing CI-tooling guidance.
- **!12913, !12914, !12915, !12916, !12917, !12918, !12919, !12921, !12924, !12925, !12926, !12927, !12928, !12930, !12931, !12932, !12933, !12935, !12938, !12939, !12940, !12942, !12943, !12944, !12945, !12948, !12949, !12950, !12952, !12953, !12954, !12956, !12959** were also reviewed from their corpus metadata, discussions, and diffs as applicable. They were primarily protocol additions/fixes, release backports, cleanup, packaging/UI maintenance, performance table changes, or straightforward correctness changes that did not justify duplicating stronger notebook conventions already established elsewhere.

## Frontier

`!12912` (`ISOBUS: change to value_string_ext for faster lookup`) exists in the same corpus commit and is merged. It was inspected only to verify that the corpus continues below this batch and is **not** part of the reviewed set above.
