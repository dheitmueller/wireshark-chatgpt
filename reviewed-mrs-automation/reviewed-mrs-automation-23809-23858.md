# Wireshark MR Review Automation: !23809–!23858

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Before selecting this batch, the already-reviewed set was rebuilt from all available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the per-run ledgers under `reviewed-mrs-automation/`. Individual MR entries were used rather than treating filename ranges as complete coverage. The historical !17571–!17620 batch remains explicitly preserved and counted.

Exactly 50 previously unreviewed corpus MRs were reviewed, in descending order. The exact set is:

!23858, !23857, !23856, !23855, !23854, !23853, !23852, !23851, !23850, !23849,
!23848, !23847, !23846, !23845, !23844, !23843, !23842, !23841, !23840, !23839,
!23838, !23837, !23836, !23835, !23834, !23833, !23832, !23831, !23830, !23829,
!23828, !23827, !23826, !23825, !23824, !23823, !23822, !23821, !23820, !23819,
!23818, !23817, !23816, !23815, !23814, !23813, !23812, !23811, !23810, !23809.

## Review weighting and durable findings

- **!23846 — Deep, merged.** BLF interface mapping assigned an enum/status return directly to `bool`, but `WTAP_OPTTYPE_SUCCESS` is zero, inverting success. Guy Harris explicitly called out the status domain and suggested retaining `wtap_opttype_return_val` or comparing to `WTAP_OPTTYPE_SUCCESS`. Promoted to `c-type-conventions.md`: status/errno-style returns are not booleans and must be compared explicitly when converted to predicates.
- **!23844 — Deep, merged.** John Thacker fixes Fibre Channel reassembly that was conditional on `tree != NULL`. Tree-less first-pass dissection must still populate state needed by redissection. Promoted to `stateful-reassembly-conventions.md`: presentation-tree availability must not gate reassembly or persistent protocol-state transitions.
- **!23816 — Deep, merged.** Gerald Combs adds `CMakePresets.json` and switches repeated CI generator/ccache command lines to `cmake --preset=ninja_ccache`, leaving job-specific options at the call site. Promoted to `build-conventions.md` as shared build-configuration guidance.
- **!23836 — Discussion-focused, closed predecessor.** Michael Mann explicitly rejected an MR sourced from the contributor's `master` branch and required a new MR from a different branch. The corrected successor !23839 merged. This strongly corroborates the notebook's already-established dedicated topic-branch submission rule; because the rule already exists and this MR was closed, no duplicate convention was added.
- **!23842 — Deep, merged.** Michael Mann removes redundant subdirectory include-directory exposure that duplicated parent-level CMake usage requirements and caused `INTERFACE_INCLUDE_DIRECTORIES` source-path errors. This corroborates existing target-oriented CMake ownership/propagation guidance rather than adding another rule.
- **!23832 — Discussion-focused, merged.** During the Open vSwitch Generic Netlink work, Alexis La Goutte asked whether the new dissectors could be combined; Jaap Keuter supported separate files because each is its own protocol. Useful architecture evidence for protocol-aligned source boundaries, but not promoted because existing architecture/source-layout guidance already captures semantic ownership and modularity.
- **!23858 / !23849 / !23848 / !23847 / !23840 / !23838 / !23837 / !23834 — Deep/corroboration, merged recursion-hardening family.** Multiple Fibre Channel SWILS, Monero, and BitTorrent DHT fixes add explicit dissection-depth checks. The DHT rationale is especially useful: even a bounded UDP datagram can contain a recursively nested structure deep enough to exhaust the C stack. Strongly reinforces `parser-recursion-conventions.md`; no duplicate rule added.
- **!23855 — Deep/corroboration, merged.** John Thacker validates WINS replication message lengths before advancing offsets after fuzzing exposed hostile 32-bit lengths. Reinforces checked-length and offset-safety guidance.
- **!23833 / !23831 / !23829 — Deep/corroboration, merged.** SMB2 offset arithmetic is moved to `ckd_add`-style checked arithmetic so overflow behavior is explicit and non-UB. Reinforces `arithmetic-safety-conventions.md`.
- **!23828 / !23819 — Deep/corroboration, merged.** PROFINET cleanup removes impossible/null checks after initialization guarantees and makes a translation-unit-local global `static`; reinforces static-analysis and linkage-intent guidance.
- **!23822 / !23813 — Deep, merged tooling robustness.** BACnet vendor generation tolerates malformed/incomplete upstream rows and parses IDs as integers rather than assuming every external table row is complete. Useful evidence for defensive generated-data tooling, but not distinct enough to warrant a new notebook rule in this batch.
- **!23814 — Discussion-focused, merged.** Anders Broman requested using an unsigned type directly rather than retaining a cast around a value that is semantically unsigned. Useful type-cleanliness evidence; existing type guidance is sufficient.
- **!23823 — Discussion-focused, merged.** The mcdata parser deliberately does not return an offset from a helper when unknown message types mean the caller cannot know a complete consumed length. Useful local API-contract reasoning, but too protocol-specific for promotion.
- **!23857 / !23830 — Scanned/corroboration, merged.** Static-analysis annotation/initialization fixes reinforce the existing policy of resolving genuine analyzer findings without distorting established semantics.
- **!23856 / !23853 / !23843 / !23809 — Scanned, merged BLF work.** Interface mapping, status-return, helper, and file-start-time changes were inspected. Only !23846 produced a new cross-cutting rule.
- **!23815 — Deep, merged.** TLS conversation/deinterlacing regression and Follow TLS Stream repair was inspected as state/redissection evidence; no new convention beyond existing conversation/state modeling guidance was required.
- **!23850 / !23852 / !23825 — Scanned, merged GUI work.** Sidebar initialization, accessibility, and plot navigation changes had no distinct cross-cutting review convention.
- **!23854 / !23845 / !23841 / !23839 / !23835 / !23827 / !23826 / !23824 / !23821 / !23820 / !23818 / !23817 — Scanned.** Protocol refinements, registrations, packaging, generated-code naming, and stable backports were inspected; they were either protocol-specific or corroborated existing notebook guidance.
- **!23812 / !23811 / !23810 — Scanned, merged automatic data updates.** Generated registry/vendor updates were inspected; !23812's malformed BACnet input led to !23813/!23822, whose durable tooling lesson is summarized above.

Merged master changes were weighted more heavily than stable backports and the closed predecessor !23836. Guy Harris's direct review in !23846 was given especially high weight. John Thacker's merged correctness work in !23844, the recursion family, and the checked-length/arithmetic fixes was also treated as strong evidence. No contrary higher-authority review was found in the batch.
