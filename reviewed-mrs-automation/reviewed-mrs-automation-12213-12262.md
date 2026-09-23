# Automated MR review — !12262 through !12213

Corpus: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Review direction: descending from the highest-numbered previously unreviewed MR.

Before selecting this batch, the already-reviewed set was reconstructed from the available `reviewed-mrs-automation/` per-run ledgers, `reviewed-mrs.md`, and other review-tracking material in the notebook. Numeric interval coverage was not assumed. The historical !17571-!17620 batch remains preserved and counted. The previously inspected !12262 frontier probe was not treated as a review until this run.

Exactly 50 MRs were reviewed in this run:

`!12262, !12261, !12260, !12259, !12258, !12257, !12256, !12255, !12254, !12253, !12252, !12251, !12250, !12249, !12248, !12247, !12246, !12245, !12244, !12243, !12242, !12241, !12240, !12239, !12238, !12237, !12236, !12235, !12234, !12233, !12232, !12231, !12230, !12229, !12228, !12227, !12226, !12225, !12224, !12223, !12222, !12221, !12220, !12219, !12218, !12217, !12216, !12215, !12214, !12213`

Status summary: 48 merged; 2 closed/unmerged (!12220 and !12217). Closed/superseded work was down-weighted relative to merged master and maintained-branch changes.

## Durable notebook findings

- **!12228 / !12229 — runtime capability versus compatibility stubs.** Merged master !12228 was authored by Guy Harris and documents macOS 14 system-libpcap remote-capture routines that are linkable but implemented as `not supported` stubs. The accepted code chooses the specialized remote API only for `rpcap://` devices, keeps local capture on the local API, and turns the generic stub failure into a capability-specific diagnostic. This was added to `platform-capability-detection-conventions.md` with very high authority; !12229 is the release backport.
- **!12256 — path quoting should become regression coverage.** John Thacker's merged CMake fix preserves build/source paths containing spaces in embedded compiler options. Gerald Combs explicitly asked for CI coverage; John agreed and noted how easily such quoting regresses. This was added as provenance to `path-and-buffer-conventions.md`, complementing later !12266's spaces-and-emoji CI build directory.
- **!12239 / !12254 — registered-field decoding and derived values.** !12239 extends packed-BCD decoding so endian semantics can be expressed by standard `ENC_*` flags, enabling later declarative dissector cleanups. In !12254 Martin Mathieson's review led SOME/IP-TP toward `proto_tree_add_item_ret_uint()` for the encoded masked value while the protocol-defined scaled offset remains a separate generated value. `field-decoding-api-conventions.md` now distinguishes registered-field decoding from additional protocol-level derivation.
- **!12248 — semantic UI values must not be combo-box row indexes.** John Thacker's merged Qt change intentionally breaks `index == enum` correspondence when two widgets expose different subsets of one enum. The stable enum is kept as item data and mapped with `findData()` / `currentData()`. This produced `ui-option-value-conventions.md`.
- **!12230 — exercise assertions in behavioral CI.** João Valverde's merged CI change moves the test suite to a Debug build so assertions and runtime checks can fire, while recognizing optimization-specific warning coverage as a separate build dimension. This produced `ci-test-configuration-conventions.md`.
- **!12260 — shared parser mechanics do not imply shared field namespace.** Peter Wu's substantive review of the merged TLS/DNS Encrypted Client Hello work called out DNS `hf_` ownership leaking through a shared HPKE helper when used from TLS. This strongly corroborates the existing `dissector-conventions.md` rule: share neutral parsing/value tables, but pass caller-owned field registrations so display-filter identity remains with the protocol that exposes the structure.
- **!12243 — option type tags describe representation, not semantic option identity.** Guy Harris's authored-and-merged comment correction clarifies that `WTAP_OPTTYPE_*` names are types used to represent options; they are not identifiers for specific option numbers. Retained as high-authority Wiretap terminology guidance without creating a new rule file.
- **!12213 — deprecate ambiguous filter syntax before removal.** The merged display-filter change warns when value strings are used without quotes because such tokens are ambiguous with protocol names/reserved words. Retained as language-evolution evidence: provide a targeted migration warning and explicit replacement syntax before removing tolerated ambiguous grammar.

## Per-MR review record

| MR | Status | Review |
| --- | --- | --- |
| !12262 | merged | Release-branch fuzz CI timing adjustment; operational CI tuning, no separate coding convention. |
| !12261 | merged | Companion release-branch fuzz CI timing adjustment; no distinct durable rule. |
| !12260 | merged | Deep review of TLS/DNS Encrypted Client Hello implementation and discussion. Strongly reinforces protocol ownership of `hf_` namespaces when parser helpers are shared. |
| !12259 | merged | iperf3 UDP loss/out-of-order detection rework; accepted behavioral correction, mainly protocol-specific. |
| !12258 | merged | Release backport correcting IEEE 1905 true/false-string semantics for BSS Configuration Report fields. |
| !12257 | merged | Companion release backport for the IEEE 1905 true/false-string correction. |
| !12256 | merged | Deep review. CMake path quoting fix with Gerald Combs request for CI regression coverage; promoted into path/tooling conventions. |
| !12255 | merged | wsutil include/dependency cleanup; localized maintenance change. |
| !12254 | merged | Deep review. SOME/IP-TP field extraction discussion distinguishes encoded masked values from the separately scaled semantic offset; promoted into field-decoding guidance. |
| !12253 | merged | `check_tfs` learns to ignore disabled `#if 0` regions; tooling-specific parser correction. |
| !12252 | merged | Wiretap declaration cleanup; maintenance-only. |
| !12251 | merged | iperf3 duplicates a cookie before storing it as a long-lived map key, corroborating lifetime-aware ownership rules. |
| !12250 | merged | `check_tfs` diagnostics/error handling cleanup; tooling-specific. |
| !12249 | merged | Automated/generated update; no new reusable review convention. |
| !12248 | merged | Deep review. Qt settings persist semantic enum data rather than combo-box indexes; promoted into UI option-value conventions. |
| !12247 | merged | Automated/generated update; no distinct convention. |
| !12246 | merged | Automated/generated update; no distinct convention. |
| !12245 | merged | Pointer/style cleanup; no cross-cutting lesson beyond normal style consistency. |
| !12244 | merged | Test/default-option argument fix; localized test harness maintenance. |
| !12243 | merged | Guy Harris authored/merged Wiretap comment correction clarifying option-type versus option-identity semantics; retained as high-authority terminology guidance. |
| !12242 | merged | C99/UAT modernization; straightforward language/style maintenance. |
| !12241 | merged | iperf2 header-condition correction; protocol-specific. |
| !12240 | merged | Logray update/integration maintenance; no general Wireshark convention extracted. |
| !12239 | merged | Deep review. Adds explicit endian support for packed BCD and audits users; promoted as foundational evidence for declarative registered-field decoding. |
| !12238 | merged | Large iperf3 dissector addition. Review feedback was useful for normal include/template/API hygiene; later fixes in this batch were weighted as stronger evidence for specific defects. |
| !12237 | merged | X.509 generated-field/value mapping correction; generated/protocol-specific. |
| !12236 | merged | SOME/IP-TP flag/parsing correction; reinforces keeping wire semantics exact but adds no separate rule beyond !12254. |
| !12235 | merged | Statistics code initializes a previously uninitialized member; ordinary correctness fix. |
| !12234 | merged | IEEE 802.11 dead-code cleanup; maintenance-only. |
| !12233 | merged | Qt class-member initialization fix; ordinary defensive initialization. |
| !12232 | merged | IPv6 address hashing uses the complete 128-bit address; correctness fix ensuring hash representation matches equality semantics. |
| !12231 | merged | UCI dead-code/handling cleanup; localized maintenance. |
| !12230 | merged | Deep review. MR tests move to Debug so assertions/runtime checks execute; promoted into CI test-configuration conventions. |
| !12229 | merged | Maintained-branch backport of the macOS Sonoma libpcap compatibility-stub fix from !12228; strengthens confidence. |
| !12228 | merged | Deep, very high-authority Guy Harris master fix. Runtime-exported libpcap symbols are compatibility stubs, so semantic capability must be checked rather than inferred from linkability; promoted into platform-capability conventions. |
| !12227 | merged | DHCPv6 correction for a failed dissector-name/assertion path; localized robustness fix. |
| !12226 | merged | Accepted TS 32.423 alternate-format parser; supersedes closed !12220 and uses explicit captures for source/destination address/port data. |
| !12225 | merged | C99 modernization; no distinct architecture lesson. |
| !12224 | merged | C99 modernization; no distinct architecture lesson. |
| !12223 | merged | GitLab pytest color/output adjustment; CI presentation only. |
| !12222 | merged | SMPP-over-TLS dissection registration (Decode As / TLS entry path); protocol integration feature, no new cross-cutting convention. |
| !12221 | merged | wsutil C99 modernization; maintenance-only. |
| !12220 | closed/unmerged | Earlier TS 32.423 alternate-format attempt. Down-weighted as superseded by merged !12226; not treated as accepted implementation precedent. |
| !12219 | merged | Debian symbol/package maintenance; no code-architecture lesson. |
| !12218 | merged | DCE/RPC IWbemLevel1Login support; feature-specific. |
| !12217 | closed/unmerged | Explicit draft CI experiment (`Test CI - do not commit`); essentially no positive precedent weight. |
| !12216 | merged | Tooling/comment cleanup around `aqtinstall`; no reusable rule. |
| !12215 | merged | XOR manpage/documentation update; localized documentation maintenance. |
| !12214 | merged | C99 modernization in ftypes; no separate lesson. |
| !12213 | merged | Display-filter parser warns on unquoted value strings before future syntax removal; useful deprecation/migration precedent, retained in this ledger. |

## Frontier

`!12212` exists in the same corpus commit, is merged, and was inspected only to prove that the corpus continues. It was **not** counted as reviewed in this run and is the next descending candidate unless a newly scraped, higher-numbered unreviewed MR appears.
