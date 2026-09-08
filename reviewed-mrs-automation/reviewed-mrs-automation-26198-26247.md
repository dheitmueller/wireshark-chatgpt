# Supplemental Reviewed Wireshark Merge Requests — !26198–!26247

This is a continuation of the reviewed-MR ledger. Consult it together with `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the earlier continuation ledgers when selecting unreviewed MRs.

## Corpus provenance

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit used for this entire review batch: **`9e52bc78659a888d4eb624984ee1a886a40d959f`**
- Exactly fifty previously unreviewed MRs were reviewed: **!26198 through !26247 inclusive**.
- No MR outside that range was counted as reviewed in this batch.

## Weighting

Merged master-branch implementation and substantive maintainer review received the greatest weight. Stable-branch cherry-picks were treated as corroboration rather than independent architectural evidence. Closed/superseded work was down-weighted as implementation evidence, but authoritative maintainer comments inside those MRs were retained when a later merged implementation or current notebook guidance corroborated them. Guy Harris's architectural feedback in !26224/!26229 and !26228/!26235 was therefore weighted substantially more highly than the abandoned implementations surrounding it.

| MR | State | Weight | Durable finding or disposition |
|---|---|---|---|
| !26198 | merged | Medium | PEAK TRC: guard optional byte-vector presence and minimum element count before indexing. Corroborates exact-object null/bounds checks. |
| !26199 | merged | Medium | PEAK cleanup: ownership applies on debug-only paths and early returns too; free match fetches and temporary column text. |
| !26200 | merged stable backport | Low/corroborative | Backport of the PEAK leak fixes; no independent rule promoted. |
| !26201 | merged | High | JSON duplicate-key tracking must use logical string equality, not pointer identity, because logically equal field names do not always share the same address. |
| !26202 | merged | High | UDX statistics need protocol stream identity finer than the UDP tuple; tap state requires explicit finish/reset ownership; tests verify stream split, totals, preference fallback, and leak cleanliness. |
| !26203 | merged | High review | Warning cleanup exposed a potential endless loop: loop counter/bound types must not permit wraparound to defeat termination. Jaap Keuter review; Martin Mathieson implementation. |
| !26204 | merged | Medium | PDCP-NR control-PDU extension; protocol-specific, no new broad convention. |
| !26205 | merged stable maintenance | Low | Dependency refresh/backport; no independent convention. |
| !26206 | merged | High | Remove redundant terminal writes; bounded string truncation must preserve UTF-8 code-point boundaries rather than blindly overwriting the final byte. |
| !26207 | merged stable maintenance | Low | Dependency update with platform compatibility constraints; useful corroboration that dependency versions must account for consumers/DLL naming. |
| !26208 | merged stable backport | Low/corroborative | DICOM overflow fix backport. |
| !26209 | merged stable backport | Low/corroborative | DICOM overflow fix backport. |
| !26210 | merged | Very high | John Thacker: extcap control capabilities are negotiated explicitly; only send `SP_QUIT` when advertised, with conservative backward-compatible inference/fallback. Promoted to `extcap-runtime-conventions.md`. |
| !26211 | merged | Very high | UDX: make ACK/reassembly transitions consumable under SACK, cumulative ACK, retransmission, loss and reorder; distinguish first observed from lowest sequence; attribute RTT to the sending flow; validated against libudx. Promoted to `stateful-reassembly-conventions.md`. |
| !26212 | merged | Medium | Headers should include the project header required for the fixed-width types they expose so they compile portably/self-sufficiently. |
| !26213 | merged | High | Preference storage should have one owner after migration; bootstrap preferences that decide whether a subsystem loads must remain available before that subsystem. |
| !26214 | merged | Low-medium | Python ruff cleanup; mostly mechanical style/tooling. |
| !26215 | merged | High | DICOM: arithmetic/comparisons must work under 32-bit integer ranks; practical operation limits are preferable to theoretical type maxima for huge exported objects. |
| !26216 | merged | Very high | John Thacker: target OS does not imply dependency/package layout; condition CMake searches on the actual repository/toolchain model. Promoted to `build-conventions.md`. |
| !26217 | merged stable backport | Low | Protocol fix backport; no independent convention. |
| !26218 | merged | High | New GUE dissector uses standard `ip.proto` dispatch, expert diagnostics, representative capture/tests, `check_dissector.py`, and fuzz validation. Strong corroboration of existing submission/dispatch rules. |
| !26219 | merged | High | Gerald Combs: extcap saved aliases/bookmarks use extensible persistent metadata rather than overloading transient capture arguments; also exposes preference-migration direction. |
| !26220 | merged | Medium-high | Remove obsolete `-municode` once the entry-point/runtime assumption requiring it is gone; toolchain flags must match current entrypoint semantics. |
| !26221 | merged | Medium-high | Extcap counterpart to !26220; corroboration rather than separate rule. |
| !26222 | merged | Medium | macOS CI explicitly installs `jsonschema` required by theme tests; CI test-gating dependencies must be provisioned rather than assumed present. |
| !26223 | merged | Very high | NVMe/MCTP: complete state identity includes tag owner; conservative subdissector handoff; missing captured bytes must yield unverified MIC rather than false verdict; keep raw data visible if structured decoder renders nothing. Existing notebook already captured these rules. |
| !26224 | closed/superseded | Very high review, low implementation | Guy Harris rejected pointer-identity guessing of a dissector `data` argument and required different entry points calling common code; superseded by !26229. Existing notebook already records this. |
| !26225 | merged stable backport | Low/corroborative | DICOM 32-bit/practical-size-limit backport of !26215. |
| !26226 | merged stable backport | Low/corroborative | DICOM 32-bit/practical-size-limit backport of !26215. |
| !26227 | merged | High | Windows extcap broken/not-connected pipes remove their event source promptly; pending/incomplete I/O retries. Corroborates existing lifecycle/error classification. |
| !26228 | closed | Very high Guy review, low implementation | Guy Harris rejected Qt regex post-processing for address masking; semantic masking belongs where typed protocol fields are available, not after rendering. Also called out unresolved conflict markers. |
| !26229 | merged | Extremely high | Guy Harris authored accepted IEEE 802.15.4 design: separate dissector entry points for distinct `data` contracts, shared parsing underneath. Existing architecture/review notebook already records it. |
| !26230 | merged | Very high | Martin Mathieson notes lossy packed RLC-NR reassembly identity and points toward a structured persistent key/equality function. Promoted with !26223 to `stateful-reassembly-conventions.md`. |
| !26231 | merged | High | Gerald Combs: warnings/errors should use a neutral tone rather than emphatic/exclamatory punctuation. |
| !26232 | merged | Very high | John Thacker centralizes extcap framed writes in one runtime owner; UI enqueues ref-counted immutable messages; queue lifetime is independent and FIFO startup is nonblocking/retried. Promoted to `extcap-runtime-conventions.md`. |
| !26233 | merged stable maintenance | Low | c-ares dependency backport; no independent convention. |
| !26234 | merged | High | SPDY custom reassembly hardened with checked aggregate/copy arithmetic; explicitly documents custom lifetime/leak weaknesses that motivate !26240. |
| !26235 | closed | Very high Guy review, low implementation | Guy repeats field-type/epan-layer masking guidance and asks for concrete evidence of claimed build failures. Closed implementation not treated as accepted architecture. |
| !26236 | merged | High | Remove development-era DLC data-type preference/heuristic once protocol-defined CVG dispatch makes it obsolete; avoid retaining user knobs that override settled protocol semantics. |
| !26237 | merged stable backport | Low/corroborative | SPDY overflow fix backport. |
| !26238 | merged stable backport | Low/corroborative | SPDY overflow fix backport. |
| !26239 | merged | Medium | Martin Mathieson: format-string rewrites in code generators must account for literal braces; concatenation can be safer than escaping generated syntax. |
| !26240 | merged | Very high | John Thacker replaces bespoke SPDY reassembly with Wireshark's standard reassembly API, gaining lifecycle correctness, dependency/frame links and avoiding wrapper leaks. Promoted to `stateful-reassembly-conventions.md`. |
| !26241 | merged | Extremely high policy | Gerald Combs tightens `SECURITY.md`: reports should be concise and reproducer files should have unique descriptive names. Promoted to `security-reporting.md`; John Thacker approved. |
| !26242 | merged | Medium | Coverity-driven extcap null guard; corroborates exact lookup-result validation. |
| !26243 | merged | High | Broad SAP hardening: early short-header checks, containing-structure bounds, bounded NUL scans, clamped lengths, subset tvbs, expert malformed diagnostics, standard bitmask helpers. Strong corroboration of existing parser rules. |
| !26244 | merged | Very high | John Thacker: checked fragment offset+length and aggregate length, invariant assertions, valid fallback tvb on error; also reinforces standard reassembly over bespoke fragment lists. |
| !26245 | open | Low/provisional | DECT NR TAP TLVs. Stig Bjørlykke asks to avoid whitespace churn because it harms review and to remove unused code. Implementation remains open/unresolved, so not treated as accepted convention evidence beyond review-process corroboration. |
| !26246 | closed | Low implementation / high review corroboration | Stig requests file-local context idiom, a descriptive commit message, and a sample capture; contributor supplied a capture. Closed work down-weighted; submission guidance corroborates existing rules. |
| !26247 | closed/superseded | Low | RTPS vendor-ID addition was superseded; no independent convention promoted. |

## Notebook updates promoted from this batch

- `stateful-reassembly-conventions.md` (new): complete structured identity keys (!26223/!26230), robust state transitions and differential validation (!26211), and preference for Wireshark's standard reassembly API over bespoke fragment accumulation (!26234/!26240/!26244).
- `extcap-runtime-conventions.md`: explicit capability negotiation for evolving control protocols (!26210) and single-owner queued control-channel writes with explicit queue lifetime (!26232).
- `build-conventions.md`: detect the actual dependency/toolchain packaging contract rather than using target OS as a proxy (!26216).
- `security-reporting.md`: concise descriptions plus unique descriptive reproducer filenames (!26241).

High-authority lessons from !26224/!26229 and several !26223 rules were already present in `architecture.md` / `review-patterns.md`, so they were deliberately not duplicated. Broad parser-hardening work in !26243 and multiple stable backports were retained as corroboration rather than expanding already well-covered guidance.