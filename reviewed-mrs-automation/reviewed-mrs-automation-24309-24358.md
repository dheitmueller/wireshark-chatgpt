# Wireshark MR automation review: !24309–!24358

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to older
MRs reviewed in this run: **50**
Historical reviewed batch !17571–!17620: **preserved and counted in the already-reviewed set**

Before selecting this batch, review tracking in `reviewed-mrs.md`, the aggregate automation tracking, and per-run files under `reviewed-mrs-automation/` was consulted. Selection was based on individual MR numbers already recorded, not on assuming that filename/range boundaries implied full coverage. The preceding completed batch ended at !24359, and neither endpoint of this candidate block appeared in prior notebook tracking; the fifty corpus records below were then fetched and reviewed individually.

## Exact reviewed set

| MR | Weight/state | Review disposition |
|---|---|---|
| !24358 | Merged | RTPS participant-index/transport inference correctness; accepted targeted fix, no new standalone rule. |
| !24357 | Merged, John Thacker | Removes mutable global GSM-RP tree context and derives context at the call site. Strong corroboration of existing per-call/re-entrant state guidance. |
| !24356 | Open snapshot | Qt inactive-interface filtering proposal; unmerged/failed pipeline, down-weighted. |
| !24355 | Merged, John Thacker | Bounds stateful WebSocket permessage-deflate output. Strong corroboration of existing bounded-decompression guidance. |
| !24354 | Open snapshot | LTE/NR Radio Bearers viewer; reviewers raised generated-file, multi-UE, maintenance, and scope concerns. Down-weighted. |
| !24353 | Merged | GSM A emergency-category bitfield correction; protocol-local fix. |
| !24352 | Open snapshot | `make-regs.py` registration deduplication proposal; maintainer discussion questioned whether duplicates should instead be treated as errors. Down-weighted. |
| !24351 | Merged, John Thacker | Windows-only unused helper cleanup; routine portability/build hygiene. |
| !24350 | Merged | ENIP DLR support; feature work without a new general convention. |
| !24349 | Merged, John Thacker | Diameter dictionary parser initialization fix; reinforces complete initialization across structurally parallel cases. |
| !24348 | Merged, John Thacker | Lua 5.5 compatibility: sequence bookkeeping must use contiguous integer keys rather than `luaL_ref()` freelist behavior. Promoted. |
| !24347 | Merged backport | OpenFlow v5 nested-bounds/infinite-loop fix. Corroborates subset-tvb and monotonic-loop rules. |
| !24346 | Merged backport | Same OpenFlow v5 hardening for another maintained branch. |
| !24345 | Merged, Guy Harris | Renames interface APIs for semantic operation rather than `pcap_findalldevs` implementation detail. Promoted with very high weight. |
| !24344 | Merged, John Thacker | Master OpenFlow v5 nested-bounds/infinite-loop fix. Strong corroboration of existing subset-tvb guidance. |
| !24343 | Merged | RPM packaging conditional adjustment; packaging maintenance. |
| !24342 | Merged, Guy Harris | Removes dead interface-listing mode parameter, simplifies invariant path, and keeps helpers file-local. Promoted with very high weight. |
| !24341 | Merged backport | RPKI-RTR checked offset arithmetic. Existing checked-arithmetic rule. |
| !24340 | Merged backport | RPKI-RTR checked offset arithmetic on another branch. |
| !24339 | Merged backport | MBIM checked offset arithmetic. Existing rule. |
| !24338 | Merged backport | MBIM checked offset arithmetic on another branch. |
| !24337 | Merged, John Thacker | Master RPKI-RTR unknown-PDU offset overflow fix. Corroborates checked arithmetic. |
| !24336 | Merged | Master MBIM checked-add/ReportedBoundsError fix. Corroborates checked arithmetic. |
| !24335 | Merged | ORAN SE27 number-table update; protocol data maintenance. |
| !24334 | Merged | IEEE 802.11 WPS nested-length correction; reinforces deriving lengths from the current bounded view. |
| !24333 | Merged backport | OpenFlow v6 zero/too-short property-length guard. Loop-progress/bounds corroboration. |
| !24332 | Merged backport | Same OpenFlow v6 property-length guard on another branch. |
| !24331 | Merged | ENIP Ethernet Link attributes; feature extension. |
| !24330 | Merged | Automated registry/translation/number updates on master. |
| !24329 | Merged backport | Automated registry/number updates on release branch. |
| !24328 | Merged backport | Automated registry/number updates on release branch. |
| !24327 | Merged backport | Enabled Protocols user-guide screenshot/description update. |
| !24326 | Merged backport | Same user-guide update on another branch. |
| !24325 | Merged backport, John Thacker | OpenFlow v5 hardening explicitly favors subset tvbuffs for nested lengths. Existing convention strongly corroborated. |
| !24324 | Merged backport, John Thacker | Same OpenFlow v5 hardening on another branch. |
| !24323 | Merged, John Thacker | Master OpenFlow v6 too-short property-length guard. Existing loop/bounds guidance. |
| !24322 | Merged, John Thacker | GitLab CI dependency corrected so openSUSE 16 test consumes openSUSE 16 package. CI wiring fix. |
| !24321 | Merged, John Thacker | Master OpenFlow v5 infinite-loop fix; bounded subsets replace manually propagated nested lengths. Existing convention strongly corroborated. |
| !24320 | Merged, John Thacker | openSUSE 16 asciidoctor package discovery fallback; dependency maintenance. |
| !24319 | Merged | Dissector warning/script cleanup including file-local linkage and missing subtree registration. Corroboration only. |
| !24318 | Merged | Capture-interface classification/description improvements; no new general rule. |
| !24317 | Merged, John Thacker | EAP-SIM zero attribute-length guard prevents non-progressing parse. Existing loop-progress rule. |
| !24316 | Merged backport, John Thacker | Unconstrained ASN.1 open types conservatively treated as potentially cyclic to force depth protection. Promoted. |
| !24315 | Merged backport, John Thacker | Same ASN.1 open-type recursion protection on another maintained branch. Promoted/corroborating. |
| !24314 | Merged backport, John Thacker | Checked increment/decrement-by-N dissection-depth helpers and corrected generated depth accounting. Promoted. |
| !24313 | Merged, John Thacker | Qt protocol-tree palette contrast respects platform theme roles. Useful UI implementation example, not promoted. |
| !24312 | Merged, John Thacker | Frees owned Qt UI object in destructor. Routine ownership fix. |
| !24311 | Merged backport, John Thacker | Checked dissection-depth-by-N helper use on another branch. Promoted/corroborating. |
| !24310 | Merged backport, Guy Harris co-author | GeoNetworking checked offset addition. High-authority corroboration of existing checked-arithmetic guidance. |
| !24309 | Merged backport, Guy Harris co-author | Same GeoNetworking checked offset addition on another maintained branch. |

## Durable notebook changes

New `api-design-conventions.md` records two high-confidence lessons from Guy Harris's merged !24345 and !24342: name APIs after stable semantic responsibilities rather than current implementation mechanisms, and remove dead mode parameters/branches while keeping genuinely local helpers file-private.

New `wslua-conventions.md` records John Thacker's merged !24348: when code relies on Lua sequence length/order, maintain contiguous integer keys directly; `luaL_ref()` provides opaque reference allocation, not sequence semantics.

New `parser-recursion-conventions.md` records !24315/!24316 and !24311/!24314: unconstrained dynamic/open types should conservatively receive recursion protection, and the invariant-bearing dissection-depth counter should be changed only through checked helper APIs with exact paired accounting.

Merged !24355 and !24357 were intentionally not turned into duplicate notebook rules because bounded decompression and avoiding mutable cross-call parser state are already documented. Likewise !24321/!24324/!24325 and the arithmetic/zero-length fixes strongly corroborate existing subset-tvbuff, monotonic-progress, and checked-arithmetic conventions.
