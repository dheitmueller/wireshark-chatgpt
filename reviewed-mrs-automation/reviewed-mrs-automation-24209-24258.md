# Wireshark MR review run: !24209–!24258

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all available per-run ledgers under `reviewed-mrs-automation/`; then selected the 50 highest-numbered corpus MRs not in that set. The historical !17571–!17620 batch remains preserved and counted. No numeric range was assumed reviewed solely from a ledger filename.

Exactly 50 MRs were reviewed in this run, newest to oldest:

| MR | Review | Durable result |
|---|---|---|
| !24258 | Deep, merged | CIP Identity Object extension. Michael Mann review rejected byte-oriented filtering for textual language data; promoted semantic-field/filterability rule. |
| !24257 | Deep/corroboration, merged backport | IPSec length-before-allocation and `tvb_memdup()` hardening; corroborates existing pre-allocation TVB-bounds rule. |
| !24256 | Deep/corroboration, merged backport | Same IPSec hardening for another stable branch; corroboration only. |
| !24255 | Deep/corroboration, merged master | John Thacker IPSec fix validates ICV/encrypted-data relation before subtraction and uses `tvb_memdup()` so bounds checks precede allocation; already covered by notebook guidance. |
| !24254 | Deep, merged | John Thacker Snort parser fix distinguishes destination-capacity APIs such as `g_strlcpy()` from source-count APIs such as `g_strndup()`; promoted string-API contract rule. |
| !24253 | Scanned, merged | Qt keyboard-navigation/visibility improvement; useful UI maintenance but no new general convention. |
| !24252 | Scanned, merged | MKA Key Management Domain correctly represented as UTF-8; reinforces encoding-aware field presentation. |
| !24251 | Deep, merged | BT HID header regression showed that a value returned through a masked field is already the field value and cannot safely be reused as the raw byte containing sibling bitfields; promoted. |
| !24250 | Deep, merged | John Thacker bounds vendor-command/event parsing with subset TVBs and separates command/event handlers; strong corroboration of existing nested-length boundary guidance. |
| !24249 | Scanned, merged | Keeps software-update API callable when feature is disabled and guards implementation internally; portability/configuration maintenance. |
| !24248 | Scanned, merged | Qt 6.10.3 dependency/release-note update; no durable engineering rule. |
| !24247 | Scanned | Npcap 1.87 stable-branch update/backport; dependency maintenance. |
| !24246 | Scanned, merged | Qt 6 deprecation fix; API-version maintenance. |
| !24245 | Scanned, closed | Proposed Windows Debug Falco library selection fix; down-weighted because it was closed rather than merged. |
| !24244 | Deep/corroboration, merged | Broadcom vendor parser bounded with subset TVB; reinforces existing nested-PDU boundary rule. |
| !24243 | Scanned | Commit-msg hook avoids style warnings for intentionally empty messages; tooling ergonomics, no broader rule promoted. |
| !24242 | Scanned, merged | JSON Dictionary leak/error-path cleanup; reinforces ownership/cleanup discipline. |
| !24241 | Deep/down-weighted, closed draft | Plugin-interface cleanup/profile-scope proposal remained draft, conflicted, and closed; retained only as negative/low-confidence evidence. |
| !24240 | Scanned | Sidebar configuration moved to global `recent_common` because it is not profile-specific; reinforces semantic configuration scope. |
| !24239 | Scanned | Software-update behavior made independent of sidebar visibility; UI state should not disable background update checks. |
| !24238 | Scanned | Objective-C++ `.mm` files added to license checking; source-check coverage maintenance. |
| !24237 | Scanned, merged | Snort error paths free partially built rule data; corroborates cleanup-on-failure ownership guidance. |
| !24236 | Deep, merged | Guy Harris rewrote pcapng-sysdig parsing around a checked `block_remaining` budget and explicitly recommended the pattern for other pcapng block readers; promoted with very high weight. |
| !24235 | Scanned, merged | TRDP double-free/leak cleanup after variable-shadowing rename; reinforces ownership correctness and avoiding stale identifier assumptions. |
| !24234 | Scanned | OpenVPN tree-info attachment backport; presentation correctness, no new general rule. |
| !24233 | Deep/corroboration | Intel vendor parser bounded with subset TVB and command/event split; reinforces nested-PDU boundary guidance. |
| !24232 | Scanned | Intel vendor read-version single-octet field-length backport; wire field length must match the actual field, not enclosing structure length. |
| !24231 | Deep, merged | John Thacker registers generic JSON-over-HTTP heuristic disabled by default when media type is absent; promoted opt-in heuristic rule for high-false-positive generic payload detection. |
| !24230 | Scanned | Intel vendor read-version field-length master/stable counterpart; corroborates exact wire-field length semantics. |
| !24229 | Scanned | Qt sidebar/welcome-page polish; UI maintenance. |
| !24228 | Scanned | Renames a close-app/capture helper to reflect what it actually does; reinforces semantic naming but adds no new rule beyond existing API-naming guidance. |
| !24227 | Scanned, closed | Proposed removal of Stratoshark welcome page; down-weighted because closed/superseded. |
| !24226 | Scanned | Sidebar-card resizing fix; UI layout maintenance. |
| !24225 | Scanned | WSDG source/doc reference hyperlink macros; documentation tooling improvement. |
| !24224 | Deep/corroboration, merged backport | Guy Harris pcapng-sysdig underflow checks; predecessor/backport evidence for the stronger !24236 checked-budget pattern. |
| !24223 | Scanned | Removes an unused RTPS field; dead-code/schema cleanup. |
| !24222 | Scanned, merged | Script-check warning cleanup in dissectors; hygiene maintenance. |
| !24221 | Scanned | NetXray padding-length fix/backport; reinforces deriving record lengths from actual format variant. |
| !24220 | Deep, merged backport | TLS analysis falls back to libgcrypt for legacy PKCS#1 v1.5 decryption when system GnuTLS policy disables it; useful analysis-vs-protected-communications context, but too subsystem-specific for a new broad rule. |
| !24219 | Scanned | `rsa.h` Doxygen backport; documentation maintenance. |
| !24218 | Scanned | HTTP/2 24-bit frame-length fix backport; corroborates width-correct wire decoding. |
| !24217 | Scanned | HTTP/2 24-bit frame-length fix backport; corroboration only. |
| !24216 | Scanned | wsgcrypt nonce lifetime/leak backport; reinforces API ownership semantics. |
| !24215 | Scanned | NetXRay padding fix backport; corroboration only. |
| !24214 | Scanned, merged | OpenVPN tree-info attachment correctness; no new broad convention. |
| !24213 | Scanned | `wsgcrypt.h` Doxygen backport; documentation maintenance. |
| !24212 | Deep, merged | HTTP/2 frame length is 24-bit and must be returned/stored in a type that can represent it; strong corroboration of existing exact-width/wire-domain guidance. |
| !24211 | Deep/corroboration | Earlier pcapng-sysdig sanity-check implementation; superseded/refined by !24236's checked remaining-budget pattern. |
| !24210 | Deep/corroboration | TLS ECH loop must advance even for zero-sized outer-extension entries; reinforces existing monotonic parser-progress rule. |
| !24209 | Scanned, merged | SGP.22 removes registration of `ProfileInfo` as a request because it is not used that way; protocol-operation registration must reflect actual protocol semantics. |

## Promoted conventions

1. Treat a length-delimited file/block as a checked consumption budget: initialize remaining bytes from the validated container length, checked-subtract each component before reading it, and only publish the completed record after all required parts validate. !24236 carries explicit Guy Harris endorsement as a reusable pcapng pattern; !24224/!24211 corroborate the evolution.
2. Respect string API contracts: destination-size functions such as `g_strlcpy()` take total destination capacity, while bounded duplication functions such as `g_strndup()` take source-byte count and append termination. !24254.
3. Match protocol-field representation and filtering to semantics. Textual data should normally be a string field rather than a collection of filterable bytes merely because bytes are convenient to decode. !24258, based on Michael Mann review and the merged revision.
4. Returned values from masked protocol fields represent the masked field; do not reuse them as if they were the untouched raw storage for adjacent bitfields. !24251, with Martin Mathieson's review leading to the accepted minimal fix.
5. Generic heuristics with a realistic false-positive surface may be registered but disabled by default, allowing users to opt in when protocol metadata is missing. !24231, authored and merged by John Thacker.

Closed/draft MRs such as !24245, !24241, and !24227 were deliberately weighted below merged master work. Routine stable backports were used as corroboration rather than counted as independent conventions.
