# Wireshark MR review automation ledger: !23109-!23158

Reviewed against corpus commit `9e52bc78659a888d4eb624984ee1a886a40d959f` using GPT-5.6 Sol.

## Selection and tracking

Before selecting this batch, the reviewed set was rebuilt from `reviewed-mrs.md` and all available per-run files under `reviewed-mrs-automation/`. MR numbers were treated individually rather than assuming that a numeric interval was reviewed merely because a tracking file covered nearby numbers. The historical !17571-!17620 batch remains part of the reviewed set and is preserved/countable for future selection.

The fifty highest-numbered corpus MRs not already in that reviewed set were exactly:

`!23158 !23157 !23156 !23155 !23154 !23153 !23152 !23151 !23150 !23149`
`!23148 !23147 !23146 !23145 !23144 !23143 !23142 !23141 !23140 !23139`
`!23138 !23137 !23136 !23135 !23134 !23133 !23132 !23131 !23130 !23129`
`!23128 !23127 !23126 !23125 !23124 !23123 !23122 !23121 !23120 !23119`
`!23118 !23117 !23116 !23115 !23114 !23113 !23112 !23111 !23110 !23109`

Exactly 50 MRs were reviewed in this run.

## Review notes

| MR | Review | Notes |
| --- | --- | --- |
| !23158 | Scanned, merged | Automatic master data/translation update. No new convention. |
| !23157 | Scanned, merged | Automatic release-4.6 data/translation update. No new convention. |
| !23156 | Scanned, merged | Automatic release-4.4 data/translation update. No new convention. |
| !23155 | Scanned, merged | Updates Qt welcome-page Discord invite link/translations. No engineering convention. |
| !23154 | Scanned, merged backport | release-4.4 backport of !23143's 802.11 variable-cardinality hardening. Same lesson as master. |
| !23153 | Scanned, merged | Fully initializes copied stat-tap UI item arrays for Coverity. Reinforces initialization hygiene. |
| !23152 | Scanned, merged backport | release-4.6 backport of !23143's 802.11 hardening. Same lesson as master. |
| !23151 | Deep, merged | John Thacker replaces hand-rolled TvbRange string-size/encoding logic with `tvb_strsize_enc()` and translates TVB exceptions into Lua errors/tracebacks. Strong helper-reuse and language-boundary exception handling; primarily corroborates existing central-helper guidance. |
| !23150 | Scanned, merged | `.gitignore` build-output cleanup. Repository hygiene only. |
| !23149 | Scanned, merged | Makes `show_exception` public header dependency more explicit/self-contained. Reinforces header-contract hygiene. |
| !23148 | Scanned, merged | Qt Coverity null-check cleanup. No new convention. |
| !23147 | Deep, merged | John Thacker adds a specific scanner error for EOF after display-filter layer `#` plus syntax regression tests. Together with !23146, establishes precise parser state/errors as input to syntax-aware UI. Promoted. |
| !23146 | Deep, merged | John Thacker fixes DisplayFilterEdit completion to follow grammar/compiler state: empty preamble or `DF_ERROR_UNEXPECTED_END` may accept a field; other failures and complete expressions do not. Promoted to `ui-dissection-conventions.md`. |
| !23145 | Scanned, merged | Qt preference-module Coverity NULL-dereference cleanup. No new convention. |
| !23144 | Scanned, merged | Frees allocated list returned by ExtArgMultiSelect walker even when result isn't otherwise consumed. Reinforces ownership/lifetime guidance. |
| !23143 | Deep, merged | John Thacker OSS-Fuzz fix replaces a 16-entry stack array with packet-scope dynamic string construction while processing an unbounded-to-end-of-TVB sequence, also eliminating possible uninitialized-value use. Strong hostile-input/cardinality evidence; existing bounds/resource guidance already covers it. |
| !23142 | Scanned, merged | New bool-return/unsigned-offset `tvb_find_tvb_remaining` API and caller conversions. Reinforces existing unsigned-domain/sentinel guidance. |
| !23141 | Scanned, merged | Adds missing T.38 ASN.1 include guards. Header hygiene. |
| !23140 | Scanned, merged | Removes empty ASN.1 dissector headers that exported nothing. Reinforces minimizing unnecessary interface surface. |
| !23139 | Scanned, merged | Moves handoff/registration declarations from headers into source where registration machinery makes external prototypes unnecessary. Reinforces narrow linkage/interface surface. |
| !23138 | Scanned, merged | Adds CAPWAP Image Identifier element dissection. Straightforward protocol coverage. |
| !23137 | Scanned, merged backport | Removes redundant Chocolatey doc dependencies from Windows GHA. No new lesson beyond dependency-source cleanup. |
| !23136 | Scanned, merged | Stops reinstalling CMake through Chocolatey because runner already supplies it. CI dependency cleanup. |
| !23135 | Scanned, merged backport | Avoids installing Perl for builds that do not regenerate code/run Perl scripts. Existing minimal-toolchain principle. |
| !23134 | Discussion-focused, merged | Windows build documentation simplification, merged by Gerald Combs. Useful documentation maintenance but no new coding/architecture rule. |
| !23133 | Scanned, merged | Master version of redundant Chocolatey asciidoctor/docbook removal. No additional lesson. |
| !23132 | Scanned, merged | Adds extcap multiselect search UI. Feature addition without durable review guidance. |
| !23131 | Scanned, merged | Bundled libpcap header update to 1.10.6. Dependency maintenance. |
| !23130 | Scanned, merged | Npcap dependency update to 1.86. Dependency maintenance. |
| !23129 | Discussion-focused, merged | Moves winbisonflex acquisition into CMake external artifacts, eliminating the last Chocolatey build dependency and enabling simpler Windows setup. Reinforces reproducible/build-owned dependency acquisition; no distinct new rule promoted. |
| !23128 | Scanned, merged | Extends `check_static.py` reference checks to event-dissectors object file. Static-check coverage maintenance. |
| !23127 | Scanned, merged | Spelling cleanup. |
| !23126 | Scanned, merged | Broad `expert_remaining`/unsigned-offset conversion. Reinforces existing TVB unsigned-domain guidance. |
| !23125 | Discussion-focused, open snapshot | eCPRI proposal to dispatch payload through a dissector table. Unmerged/open, so treated as provisional and not used as accepted architecture evidence. |
| !23124 | Scanned, merged backport | Stable-branch backport of !23110 sharkd async DNS fix. Reinforces the master architectural conclusion. |
| !23123 | Scanned, merged | GTP optional-byte/gap dissection correction. Reinforces exact offset/optional-field accounting. |
| !23122 | Scanned, merged | Converts TVB base64 APIs to unsigned offsets/lengths. Reinforces existing API-domain guidance. |
| !23121 | Deep, merged | Jaap Keuter removes broad `wiretap/wtap_opttypes.h` inclusion from `epan/packet.h`, using an incomplete Wireshark-owned block type and requiring definition-using sources to include the defining header. Added as corroboration/clarification to API header-dependency guidance, with explicit caveat that dependency-owned external typedefs must still come from their authoritative headers. |
| !23120 | Scanned, merged | Additional `expert_remaining` and unsigned-offset conversion. Existing rule. |
| !23119 | Deep, merged | John Thacker LoRaWAN Coverity fix moves from signed to unsigned offsets, uses add-and-return helpers to avoid duplicate fetches, and changes `remaining(offset) - 4` into `remaining(offset + 4)` to avoid underflow. Strong corroboration of existing unsigned arithmetic/fetch-once rules. |
| !23118 | Scanned, merged | Enables assertions during Coverity analysis so analyzer can use asserted invariants. Reinforces existing assertion/static-analysis guidance. |
| !23117 | Scanned, merged | Replaces `tvb_find_uint8` uses where `-1` sentinel conflicts with unsigned offsets. Reinforces existing status-plus-output/sentinel-domain guidance. |
| !23116 | Scanned, merged | Debian package naming fix (`_` to `-`). Packaging-specific. |
| !23115 | Scanned, merged | Extends repeated-fetch checking to 64-bit values. Reinforces fetch-once/static-check conventions. |
| !23114 | Scanned, merged backport | IDN array-index buffer-overrun fix. Existing exact-bounds guidance. |
| !23113 | Scanned, merged | Moves reassembled-alert migration outside an alert loop, removing O(N²) work and redundant lookup. Straightforward performance/cleanup lesson. |
| !23112 | Scanned, merged | Adds missing NULL check to match other proto paths. Defensive consistency. |
| !23111 | Discussion-focused, merged | Adds `tvb_strsize_enc()` and uses it to determine STRINGZ length without copying the whole string. Provides the centralized helper later reused by !23151; reinforces semantic helper reuse and avoiding unnecessary data copies. |
| !23110 | Deep, merged | sharkd DNS synchronization design was explicitly challenged in review. The accepted distinction is that stateful interactive sharkd can resolve asynchronously and show names on subsequent requests, while single-pass TShark has different needs; blocking every sharkd request on external DNS made it unusable. Promoted to `name-resolution-conventions.md`. |
| !23109 | Scanned, merged | Warns about encoding arguments known unsupported by relevant string APIs. Reinforces validating API-domain constraints. |

## Durable notebook changes

- `ui-dissection-conventions.md`: added parser/grammar-driven syntax-assistance rule from merged !23146 and !23147.
- `api-design-conventions.md`: strengthened narrow public/common-header dependency guidance with merged !23121 while preserving the stronger rule against locally reconstructing external-library public typedefs.
- `name-resolution-conventions.md`: added frontend-lifecycle rule from merged !23110, corroborated by stable backport !23124.

## Important corroboration intentionally not duplicated

The !23143/!23152/!23154 fuzz-hardening series, !23119 arithmetic/fetch cleanup, !23142/!23126/!23122/!23120/!23117 unsigned-TVBuff work, !23115 repeated-fetch checker, and !23118 Coverity assertion change all provide useful evidence for conventions already recorded elsewhere. They were counted as reviewed but were not used to create redundant notebook rules.
