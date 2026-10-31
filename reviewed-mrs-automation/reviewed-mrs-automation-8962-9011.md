# Wireshark automated MR review — !9011 through !8962

Reviewed **2026-10-31**, using **GPT-6**, against `dheitmueller/wireshark-corpus-mrs` at exact Git commit **`ddcaa22b51c68f594e425a23388c3a2086813054`**.
Notebook starting commit: `ac69fc14cb942d79b08d8c2a40d0f6b930628992`.

**Exact reviewed membership (50 distinct; descending):** !9011, !9010, !9009, !9008, !9007, !9006, !9005, !9004, !9003, !9002, !9001, !9000, !8999, !8998, !8997, !8996, !8995, !8994, !8993, !8992, !8991, !8990, !8989, !8988, !8987, !8986, !8985, !8984, !8983, !8982, !8981, !8980, !8979, !8978, !8977, !8976, !8975, !8974, !8973, !8972, !8971, !8970, !8969, !8968, !8967, !8966, !8965, !8964, !8963, !8962.

**Outcomes in this pinned corpus snapshot:** 46 merged; four closed/unmerged (!9004, !8998, !8982, !8970). No open MR in the batch. Historical outcomes should not be mistaken for live upstream statuses.

## Duplicate-avoidance and historical preservation

Inventoried **366** automation-tracking files under `reviewed-mrs-automation/` and `reviewed-mrs.md` (367 tracking resources total); checked aggregate tables, all 18 unusual/exact-list/noncontiguous/backfill/gap ledgers, the preceding exact-membership ledger !9012–!9061 and the low-numbered ledger coverage. The ordinary per-run ledgers' lowest named endpoint is !9012, and the exceptional and aggregate files contained no prior reviewed entry in !8962–!9011. The previous ledger mentioned !9011 solely as a **next candidate**, not reviewed membership. All fifty `mr_<iid>.json` records were retrieved from the pinned corpus. Selection used explicit candidates and exceptional memberships; it did **not** count an entire apparent range merely because some nearby numbers were in a ledger.

The historical `reviewed-mrs-automation-17571-17620.md` remains preserved: **50 rows, 50 distinct numbers, every !17571–!17620**. Original blob SHA `9393377c3adeb3a38d502532839765e3abe771fe`, unchanged by this run.

## Diff and discussion assessments

| MR | Corpus outcome | Review depth | Assessment |
| --- | --- | --- | --- |
| !9011 | merged | Discussion / diff | Qt graph reordering; Gerald flagged clutter/stray UI character |
| !9010 | merged | Scanned | port heuristic plus consistent reported length |
| !9009 | merged | Scanned | same correction in stable branch |
| !9008 | merged | Discussion / diff | typed Qt connect across widgets and models |
| !9007 | merged | Scanned | generated manufacturer/vendor/enterprise source update |
| !9006 | merged | Scanned | duplicate automated update on another branch |
| !9005 | merged | Scanned | another branch's generated source refresh |
| !9004 | closed | Deep | Tomasz SMB [Illegal Segments] regression; superseded idea, not precedent |
| !9003 | merged | Deep | bound loops and allocations; Gerald scope feedback, John referenced !8991 |
| !9002 | merged | Scanned | third-party CI action version migration |
| !9001 | merged | Discussion / diff | recognize ACK after probe and amended documentation |
| !9000 | merged | Scanned | share Common-IE definition; rebuild generated packet dissector |
| !8999 | merged | Scanned | optional semantic address details and release note |
| !8998 | closed | Discussion / diff | Alexis asked for built-in dissector; Anders deemed approach obsolete |
| !8997 | merged | Deep | Stig objections, later refactor !11757, broken docs fixed !9342 |
| !8996 | merged | Deep | post-merge null QAction / Add as Column report |
| !8995 | merged | Discussion / diff | Alexis suggests value_string/range_string instead of switch |
| !8994 | merged | Scanned | decode T=0/T=1 parameter messages |
| !8993 | merged | Deep | array terminators and duplicate fields; checker acknowledges scope limits |
| !8992 | merged | Scanned | avoid repeated output realloc-copy and preserve old data |
| !8991 | merged | Deep | one-byte malformed capture proves unconditional loop nontermination |
| !8990 | merged | Deep | searchable packet string distinct from display escape/label |
| !8989 | merged | Scanned | add structured multicast address metadata |
| !8988 | merged | Discussion / diff | unacked list beats optimistic max seq; capture order not timestamp sort |
| !8987 | merged | Scanned | dispatch V5EF in E1/T1 subtype |
| !8986 | merged | Scanned | return absolute cursor and terminate on error |
| !8985 | merged | Scanned | reject stats lengths smaller than fixed header |
| !8984 | merged | Scanned | same zero-length loop repair in stable branch |
| !8983 | merged | Scanned | diagnose compressed length zero; avoid non-progress |
| !8982 | closed | Discussion / diff | Alexis required topic branch and clean options; later !9013 |
| !8981 | merged | Scanned | check fileno before GLib color probe |
| !8980 | merged | Discussion / diff | avoid GLib assertion when std descriptor invalid |
| !8979 | merged | Discussion / diff | start only from noneditable interface columns |
| !8978 | merged | Deep | Guy Harris reported-vs-captured and length-field review |
| !8977 | merged | Deep | reject length below four-byte fixed header; Alexis cross-version check |
| !8976 | merged | Deep | return offset, mark last header on bad extension |
| !8975 | merged | Deep | third-party codec must not run on absent data |
| !8974 | merged | Scanned | use font lineSpacing consistently across Qt5/6 |
| !8973 | merged | Deep | Alexis license, Michael test vectors, John typed wmem allocation |
| !8972 | merged | Scanned | viewport metrics and consistent width naming |
| !8971 | merged | Deep | John Thacker Wayland scrolling regression; subsequent !11677 |
| !8970 | closed | Discussion / diff | Gerald withdrew: mitigation libraries not guaranteed installed |
| !8969 | merged | Discussion / diff | separate arbitrary wire name from UTF-8 field string |
| !8968 | merged | Discussion / diff | Gerald: VS2019 remained supported despite preferred docs |
| !8967 | merged | Deep | Alexis: 1/2 enum not tfs 0/1; corrected FT_UINT8 to FT_UINT16 |
| !8966 | merged | Scanned | fix missing NULL and unreachable fallback |
| !8965 | merged | Discussion / diff | replace booleans with named bitmask options |
| !8964 | merged | Deep | Gerald explains runaway-label protection vs UTF-8 truncation |
| !8963 | merged | Deep | use conv endpoint API; Anders/Gerald demand unrelated Qt split |
| !8962 | merged | Discussion / diff | always initialize caller-visible error on failure |

## Durable lessons and weighting

The strongest master-branch acceptance evidence concerns !8978 (heuristic recognition and Guy Harris's capture-length distinctions), !8973 (SAP decompression library licensing, test fixtures, wmem allocation), !8964 (UTF-8 versus implicit strbuf byte ceiling), !8990 (string values versus display labels), !8967 (actual enum values and field widths), !9003/!8991/!8977/!8976 (stream/parser loop termination), and !8997's WSLua API design debate.

**Maintainer authority:** Guy Harris's guidance about `tvb_reported_length_remaining()`, Gerald Combs's strbuf-safety intent and Qt platform constraints, Stig Bjørlykke's API consistency critique, John Thacker's decoding/Wayland/typed-allocation feedback, Michael Mann's refusal to discard a failing decoder unit test, and Alexis La Goutte's field-width/license/submission reviews were given particular weight. Criticism after merge is explicitly retained.

**Supersession / cautions:** !8997 was refined by !11757, with generated-doc failure corrected by !9342; !8971 produced a Qt Wayland regression and later !11677 was referenced; !9003's local Kafka Zstd code was subsequently improved by shared TVBuff helpers in !9060. Closed !9004/!8998/!8982/!8970 are not accepted implementation precedents. Multiple stable backports and automated registry refreshes count toward the fifty reviewed but do not multiply design evidence.

**Testing limit:** Reviewed the stored corpus JSON discussions and diffs, not live GitLab state. No local compilation, runtime testing, packet capture or fuzzing was carried out for this notebook update. Prefer reproducing critical issues with focused captures and cross-platform builds before changing current code.

**Next expected frontier candidate:** !8961, subject to full reviewed-set and corpus-tree reconciliation on the next run.
