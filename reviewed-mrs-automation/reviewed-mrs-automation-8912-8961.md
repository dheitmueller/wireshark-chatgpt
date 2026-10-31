# Wireshark automated MR review — !8961 through !8912

Reviewed **2026-10-31**, using **GPT-6**, against `dheitmueller/wireshark-corpus-mrs` pinned at exact Git commit **`ddcaa22b51c68f594e425a23388c3a2086813054`**. Notebook starting commit: `9f0ea0a33db1d28d88c0bd5d4268f5afc5fbf548`.

**Exact reviewed membership (50 distinct MRs, descending):** !8961, !8960, !8959, !8958, !8957, !8956, !8955, !8954, !8953, !8952, !8951, !8950, !8949, !8948, !8947, !8946, !8945, !8944, !8943, !8942, !8941, !8940, !8939, !8938, !8937, !8936, !8935, !8934, !8933, !8932, !8931, !8930, !8929, !8928, !8927, !8926, !8925, !8924, !8923, !8922, !8921, !8920, !8919, !8918, !8917, !8916, !8915, !8914, !8913, !8912.

**Outcomes at pinned corpus snapshot:** 46 merged, four closed and unmerged (!8958, !8953, !8926, !8923). No open MRs in the batch. These historical outcomes are not live GitLab status claims.

## Review selection and duplicate-avoidance

Inventoried the notebook's 367 existing files in `reviewed-mrs-automation/`, the root `reviewed-mrs.md`, and the supplemental aggregate `reviewed-mrs-automation/reviewed-mrs-automation.md`. Read the exceptional / exact-list / noncontiguous / skipped-number / gap / backfill ledgers, current aggregate records and latest contiguous ledger; reconciled the ordinary ledger filename spans to the corpus tree rather than counting mere cross-references as reviewed. The smallest ordinary already-reviewed range starts at !8962; the exceptional files and aggregate tracking contain no reviewed number below it. Every candidate !8961–!8912 exists in the pinned corpus; no duplicate candidates were found. Ordinary ledger membership above this frontier remains subject to explicit per-run membership (do not infer a whole range from a partial ledger).

The original !17571–!17620 ledger remains present with exactly 50 distinct reviewed rows. Original ledger blob SHA: `9393377c3adeb3a38d502532839765e3abe771fe`; no changes made to that ledger.

## Exact MR assessments

| MR | Snapshot outcome | Depth | Review assessment |
| --- | --- | --- | --- |
| !8961 | merged | deep | BPv6 error exits return absolute cursor, set lastheader to terminate |
| !8960 | merged | deep | Validate decoded CitrixAGBasic token bytes before text display; credentials tap |
| !8959 | merged | scanned | Regenerated ASN.1 outputs without default line directives |
| !8958 | closed / unmerged | discussion | H.221 manufacturer table proposal closed; review duplicate and whitespace complaints |
| !8957 | merged | scanned | Static linkage checker: keep internal symbols static |
| !8956 | merged | discussion | Accepted regex correction for checker false-positive labels, supersedes !8953 |
| !8955 | merged | deep | Generator flag -L default; opt-in ENABLE_DEBUG_A2W for line mapping |
| !8954 | merged | scanned | Check fileno before GLib color probe |
| !8953 | closed / unmerged | discussion | Closed erroneous quote fix; replaced by !8956 |
| !8952 | merged | deep | CIMD encoded user data conversion through existing charset support |
| !8951 | merged | deep | PostgreSQL error field-length decrement; sample capture and commit-message review |
| !8950 | merged | scanned | Windows CMake 3.25 library discovery stable-branch backport |
| !8949 | merged | scanned | Windows CMake 3.25 discovery stable-branch backport |
| !8948 | merged | deep | Structured dfilter error object; follow-up !8962 fixes missing error result |
| !8947 | merged | scanned | Remove arbitrary ITEM_LABEL_LENGTH clipping from AJP13 |
| !8946 | merged | deep | Remove wmem_strbuf_new_label and byte-boundary truncation |
| !8945 | merged | scanned | S1AP IE identifier fix backport |
| !8944 | merged | scanned | S1AP IE identifier fix backport |
| !8943 | merged | deep | Restore accidentally reverted S1AP cell-ID mask/decoder |
| !8942 | merged | deep | Fix S1AP IE ID, but bundled accidental revert; Pascal caught it |
| !8941 | merged | discussion | Windows MSVC ABI CMake 3.25 .a vs .lib matching |
| !8940 | merged | scanned | Automatic registry/manufacturer refresh |
| !8939 | merged | scanned | Automatic registry/manufacturer refresh backport |
| !8938 | merged | scanned | Automatic registry/manufacturer refresh backport |
| !8937 | merged | deep | Cached first gap + contiguous frontier, invalidation and capture benchmarks |
| !8936 | merged | scanned | RFC 863 Discard dissector |
| !8935 | merged | discussion | H.224 extended/nonstandard client ID fixes and base-display guidance |
| !8934 | merged | scanned | Avoid invalid UTF-8 in Bluetooth string labels |
| !8933 | merged | deep | Use full UTF-8 string for 802.11 tree field; wmem get_str vs finalize |
| !8932 | merged | scanned | Qt6 RTP playback marker after pause/resume |
| !8931 | merged | deep | Opt-in fragment_add_check_with_fallback for late retransmissions |
| !8930 | merged | scanned | libpcap module documentation URL |
| !8929 | merged | deep | SAP SNC dissector: typed fields, length diagnostics, captured-vs-reported |
| !8928 | merged | scanned | H.264/H.265 signed Exp-Golomb overflow parity |
| !8927 | merged | deep | Use tvb_new_octet_aligned for non-byte-aligned protocol addresses |
| !8926 | closed / unmerged | discussion | Closed proposed dissector status sentinel; Guy Harris distinguishes outcomes |
| !8925 | merged | discussion | New DECT dissector, static warnings, post-merge NULL array and dead-code fixes |
| !8924 | merged | scanned | Clang Analyzer dead store repair |
| !8923 | closed / unmerged | discussion | Unmerged UTF-8-specific wmem buffer; alternatives and label sizing |
| !8922 | merged | discussion | cppcheck cleanup: redundant HTTP conversation initializer side effect |
| !8921 | merged | scanned | BGP parser cursor correction backport |
| !8920 | merged | scanned | BGP parser cursor correction backport |
| !8919 | merged | deep | SIP Call-ID full value vs bounded hash lookup; warn if mandatory missing |
| !8918 | merged | discussion | H.224 Q.922 header bits and named priority values |
| !8917 | merged | deep | LAPDm fragment ID reuse with first-pass-only state and redissection semantics |
| !8916 | merged | deep | MCTP-aware vs trivially encapsulated dispatch tables |
| !8915 | merged | discussion | Qt protocol-tree root lacks field_info; avoid null dereference |
| !8914 | merged | scanned | Remove unused tfshark function |
| !8913 | merged | discussion | BGP parser reader_offset and IPv6 correction; backport |
| !8912 | merged | scanned | Windows winflexbison3 CI backport |

## Durable findings and evidence weighting

- **!8937 (merged):** Tomasz Moń's first-gap pointer and contiguous-length cache avoid repeated fragment-list traversals, but must remain synchronized through insertion, list merging, fragment truncation and deletion. John Thacker tested varied stress captures; Ivan Nardi reported major measured improvements, including a sample changing from ~20 seconds to ~1 second after later refinements. Performance numbers are contributor reports, not benchmark results from this notebook run.
- **!8931 (merged):** `fragment_add_check_with_fallback()` is an explicit alternative for USBLL late retransmissions, rather than a global reinterpretation of all completed fragment streams. Tomasz's reviews caught TCP→HTTP regressions and supplied exact public fuzz captures/packet numbers. Reassembly tests must run across consumers.
- **!8917 (merged):** Pascal Quantin establishes first-pass (`!PINFO_FD_VISITED`) versus out-of-order redissection semantics for mutable fragment state and suggests per-frame historical sequence counters for state that must survive arbitrary packet navigation.
- **!8948 (merged) plus earlier !8962 (merged):** Typed display-filter diagnostics (`df_error_t`: code, message, location, free function) are better than loose parallel out-parameters, but all failure paths must construct the error even if a fatal logger is normally expected.
- **!8946, !8933 (merged) vs !8923 (closed):** Remove arbitrary octet-limit truncation from string values; let UI truncate the displayed label safely. Avoid reallocating `wmem_strbuf_finalize()` unnecessarily if a borrowed `get_str()` suffices, while noting João Valverde's caution about pointer invalidation. The separately proposed `wmem_u8buf` is *not* an accepted API.
- **!8955/!8959 (merged):** ASN.1 `asn2wrs` source-line directives default off for normal generated output, opt in using `ENABLE_DEBUG_A2W=ON`. Regenerate authoritative outputs when changing generator flags.
- **!8929/!8925 (merged):** Martin Mathieson's typed-tree API and port-preference suggestions are strong reviewer guidance but not unconditional bans on equivalent safe patterns. Use `proto_tree_add_item_ret_uint()` where it avoids redundant reads; validate expert info against captured-versus-reported length and avoid conflicting filter names. John Thacker's post-merge NULL-terminated array finding in !8925 required corrective !8966.
- **!8926 (closed):** Guy Harris questioned overloading dissector return-value status with a magic sentinel. This was *not merged*; preserve his richer taxonomy of rejected payload, recognized zero-payload, bytes consumed, and various desegmentation needs as design guidance, not a current return API.
- **!8942/!8943 (merged):** Pascal Quantin caught an unintended rollback of an independent S1AP field fix bundled into a focused IE-ID change; the next MR repaired it. Check nearby unrelated hunks and generator/compiled-output consistency.
- **!8951 (merged):** Alexis required a capture demonstrating PostgreSQL error messages; Peter Wu supplied protocol-format and TLS-decryption reproduction details. The defect was an unconsumed field-code byte in a length-accounted loop; test both length accounting and unknown future fields.
- **!8916 (merged):** Two MCTP tables distinguish whole-payload MCTP-aware handlers from simple inner payloads that should receive a TVB subset excluding the type byte; the integrity-check bit restricts valid fallback dispatch.
- **!8953, !8958, !8923, !8926 (closed):** Do not treat these proposals as accepted implementation exemplars. Merged maintenance/backport clones and routine automatic data updates count as reviewed but add little independent design evidence.

**Testing boundary:** The 50 stored corpus JSON records, their change diffs and available non-system MR discussion comments were analyzed. No compilation, fuzzing, capture decoding or runtime verification was executed.

**Next expected frontier candidate:** !8911, subject to exact reviewed-set reconciliation and newly available corpus records.
