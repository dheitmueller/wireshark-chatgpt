# Wireshark MR Review Automation: !23909–!23958

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Before selecting this batch, the already-reviewed set was rebuilt from all available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the per-run ledgers under `reviewed-mrs-automation/`. Individual MR entries were used rather than treating filename ranges as complete coverage. Prefix checks against the tracking found no pre-existing entries in !23909–!23958; the `!2395...` tracking hit was the prior batch ending at !23959. The historical !17571–!17620 batch remains explicitly preserved and counted.

Exactly 50 previously unreviewed corpus MRs were reviewed, in descending order. The exact set is:

!23958, !23957, !23956, !23955, !23954, !23953, !23952, !23951, !23950, !23949,
!23948, !23947, !23946, !23945, !23944, !23943, !23942, !23941, !23940, !23939,
!23938, !23937, !23936, !23935, !23934, !23933, !23932, !23931, !23930, !23929,
!23928, !23927, !23926, !23925, !23924, !23923, !23922, !23921, !23920, !23919,
!23918, !23917, !23916, !23915, !23914, !23913, !23912, !23911, !23910, !23909.

## Review weighting and durable findings

- **!23934 — Deep, merged.** Public DNS tap API exposure. Michael Mann corrected the patch so `WS_DLL_PUBLIC`/Wireshark tooling, rather than a redundant manual header list, defines the public header surface, and removed an unnecessary `packet.h` dependency. Promoted to `api-design-conventions.md`.
- **!23936 — Deep, merged.** Broad consistency change puts `config.h` first and `WS_LOG_DOMAIN` immediately after it in files defining a custom log domain. Promoted to `logging-conventions.md`.
- **!23930 — Deep, merged.** K12 writer validates full variable-length record size before fixed-buffer copies and changes from `g_hash_table_foreach()` to explicit iteration so write failures can stop and propagate. Promoted to `wiretap-writer-conventions.md`.
- **!23925 — Deep, merged.** John Thacker fixes a platform assumption that `time_t` is `long`, using `intmax_t`/`%ji` and noting the existing time-format helper as an alternative. Promoted to `c-type-conventions.md`.
- **!23958 — Deep/corroboration, merged.** John Thacker and Jaap Keuter discuss moving wiretap parsing toward `Buffer` APIs; John notes that direct raw-buffer writes followed by manual `ws_buffer_increase_length()` are easy to forget. Reinforces existing Buffer/logical-length guidance rather than creating a duplicate rule.
- **!23955 — Deep/corroboration, merged.** ECH checks payload length against the algorithm's authentication-tag length before subtraction and derives subsequent allocation/copy lengths from the validated value. Reinforces checked-length arithmetic.
- **!23954 / !23950 — Deep/corroboration, merged.** Checksum API cleanup and follow-up explicitly treat an impossible flag/NULL-computed-checksum combination as programmer error worthy of `DISSECTOR_ASSERT`, corroborating the notebook distinction between programmer invariants and malformed packet input.
- **!23933 — Deep, merged.** TRDP overhaul incorporates earlier review by replacing custom XML/CRC machinery with established libraries/helpers and removing inappropriate UI-layer coupling. Strong corroboration of reuse/layering guidance; no duplicate rule added.
- **!23929 / !23927 / !23921 — Deep/corroboration, merged family.** TLS ECH hardening uses a bounded subset TVB and wider offset/end types to prevent overflow. Reinforces existing subset-TVBuff and arithmetic-domain guidance.
- **!23915 — Scanned/corroboration, merged.** An intentionally unused heuristic-dispatch return value is explicitly cast to `(void)` rather than inventing meaningless handling solely for Coverity. Useful static-analysis evidence, but not promoted as a separate rule.
- **!23957 — Discussion-focused, closed/superseded.** Static-symbol cleanup was folded into !23933 after Michael Mann asked whether it should be squashed there. Down-weighted in favor of the merged successor.
- **!23956 / !23953; !23938 / !23937; !23911 / !23910 — Scanned backport/update pairs.** Packaging, K12 hardening backports, and Sparkle dependency updates added no distinct durable convention beyond their merged master counterparts.
- **!23949 / !23948 / !23947 — Scanned, merged automatic updates.** Generated/manuf/services/translation maintenance; no reusable review rule.
- **!23952 / !23951 / !23946 / !23945 / !23944 / !23943 / !23942 / !23941 / !23940 / !23939 — Scanned.** Protocol maintenance, logging migration, and Coverity fixes were checked; they either had no substantive human-review lesson or reinforced existing null-check/static-analysis/helper-reuse guidance.
- **!23935 / !23932 / !23931 / !23928 / !23926 / !23924 / !23923 / !23922 / !23920 / !23919 / !23918 / !23917 / !23916 / !23914 / !23913 / !23912 / !23909 — Scanned.** Tool/build/protocol/crypto/static-analysis/release-note changes were inspected; no additional durable convention justified promotion beyond the rules already present or added in this run.

Merged outcomes were weighted over closed/superseded work throughout. No Guy Harris-authored or Guy Harris-reviewed MR in this exact batch produced a stronger contrary architectural signal; maintainer corrections from Michael Mann, Jaap Keuter, John Thacker, Anders Broman and others were weighted according to specificity and merge outcome.
