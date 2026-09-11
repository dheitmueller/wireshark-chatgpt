# Wireshark MR Review Automation: !23859–!23908

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Before selecting this batch, the already-reviewed set was rebuilt from all available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the per-run ledgers under `reviewed-mrs-automation/`. Individual MR entries were used rather than treating filename ranges as complete coverage. Exact/prefix checks found no pre-existing review entries for the candidates below. The historical !17571–!17620 batch remains explicitly preserved and counted.

Exactly 50 previously unreviewed corpus MRs were reviewed, in descending order. The exact set is:

!23908, !23907, !23906, !23905, !23904, !23903, !23902, !23901, !23900, !23899,
!23898, !23897, !23896, !23895, !23894, !23893, !23892, !23891, !23890, !23889,
!23888, !23887, !23886, !23885, !23884, !23883, !23882, !23881, !23880, !23879,
!23878, !23877, !23876, !23875, !23874, !23873, !23872, !23871, !23870, !23869,
!23868, !23867, !23866, !23865, !23864, !23863, !23862, !23861, !23860, !23859.

## Review weighting and durable findings

- **!23898 — Deep, merged.** Michael Mann fixes preference storage that had migrated to `wmem_epan_scope()` but was still freed/replaced with GLib allocation APIs. Persistent preference-owned strings must be released and replaced with the allocator/scope that owns them. Promoted to `preference-state-conventions.md`.
- **!23875 — Deep, merged.** Roland Knall replaces Qt Designer `on_<widget>_<signal>()` auto-connect naming with explicit `connect()` calls, citing Qt's recommendation and existing Wireshark patterns. Promoted to `platform-gui-conventions.md` as a maintainability/refactor-safety convention.
- **!23863 — Deep, merged documentation.** The Wireshark Developer's Guide now documents CMake idioms and required plugin `CMakeLists.txt` structure instead of telling contributors to cargo-cult another plugin. It explicitly documents plugin source/support lists, `add_wireshark_plugin_library`, and required `target_link_libraries` dependencies. Promoted to `plugin-build-conventions.md`; current upstream build definitions remain authoritative.
- **!23895 — Deep, merged.** John Thacker removes a dissector-local UTF-16 BOM parser and uses the existing `ENC_BOM` facility, with big-endian fallback for absent BOMs. Strong helper-reuse evidence; it corroborates existing reuse/API guidance rather than creating another rule.
- **!23894 / !23893 / !23888 — Deep/corroboration, merged recursion-hardening family.** AFP Spotlight recursion can exhaust the stack despite TCP segmentation because the PDU length is large and tree-less dissection does not naturally stop recursion. Accepted fixes use `increment_dissection_depth()` / `decrement_dissection_depth()`. Reinforces existing explicit recursion-depth guidance.
- **!23866 — Deep/corroboration, merged.** John Thacker hardens DLMS/COSEM compact-array parsing against syntactically valid but semantically inconsistent descriptions that consume zero bytes, ensures unknown types still advance, and uses a subset TVB to bound array contents. Strong corroboration of monotonic progress and bounded-subset parsing.
- **!23870 — Deep, merged.** CQL replaces hard-coded metadata skips with a recursive metadata-descriptor walker so absent/empty values advance by the real type descriptor rather than assumed fixed widths. The author validated with a capture that failed before and passed after. Reinforces protocol-structure-driven offset advancement and reproducer-based validation.
- **!23876 — Deep/corroboration, merged.** John Thacker changes editcap exit cleanup from record reset to final cleanup so error paths release the record buffer; the MR includes the ASan/LSan trace that exposed the leak. Reinforces cleanup-on-all-exits and sanitizer-driven validation.
- **!23883 — Deep/corroboration, merged.** Intentionally ignored `g_strlcpy()` returns are made explicit with `(void)` rather than inventing meaningless handling. Reinforces the established static-analysis rule: distinguish intentional ignored results from real defects and make intent visible.
- **!23859 — Deep/corroboration, merged stable backport.** Fibre Channel SWILS recursion is explicitly depth-guarded because reassembly means packet exhaustion is not a reliable recursion limit. The master/stable lineage reinforces the recursion rule; the backport itself adds no distinct convention.
- **!23908 / !23900 — Scanned, merged.** OSS-Fuzz linker parallelism is constrained according to observed memory use to avoid OOM. Useful CI capacity tuning, but too environment-specific for a durable coding rule.
- **!23907 — Discussion-focused, open draft.** Experimental generic/IPv4 fragment out-of-order detection remains unmerged and presents alternate implementations; down-weighted as provisional design work.
- **!23901 — Scanned/corroboration, merged.** Coverity-specific reassurance around a statically bounded string supports the existing policy of addressing analyzer diagnostics without distorting known-safe code.
- **!23889 — Scanned, merged.** RDM length/fallthrough follow-up requested from an earlier review; useful local cleanup, no additional cross-cutting convention.
- **!23881 — Discussion-focused, closed draft.** BLF naming experiment was closed and is down-weighted relative to merged BLF work in the batch.
- **!23906 / !23905 / !23904 / !23903 / !23902 / !23899 / !23897 / !23896 / !23892 / !23891 / !23890 — Scanned.** Build/tooling, protocol maintenance, installer, naming, and UI changes were inspected; no distinct durable convention justified promotion.
- **!23887 / !23886 / !23885 / !23884 / !23882 / !23880 / !23879 / !23878 / !23877 / !23874 / !23873 / !23872 / !23871 / !23869 / !23868 / !23867 / !23865 / !23864 / !23862 / !23861 / !23860 — Scanned.** Backports, packaging/build maintenance, protocol refinements, GUI fixes, and support changes either had no substantive human-review lesson or corroborated rules already represented in the notebook.

Merged outcomes were weighted over drafts, closed experiments, and backports. No Guy Harris-authored or Guy Harris-reviewed MR in this exact batch supplied a stronger contrary architectural signal. Accepted work from John Thacker, Michael Mann, Anders Broman, Roland Knall and other maintainers was weighted according to specificity, merge outcome, and whether the lesson was already represented in the notebook.
