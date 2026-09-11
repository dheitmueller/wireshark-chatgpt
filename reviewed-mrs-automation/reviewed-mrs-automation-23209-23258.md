# Automated Wireshark MR review: !23209-!23258

## Corpus identity

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
- Review direction: newest available previously-unreviewed MRs toward older MRs
- MRs reviewed in this run: **50**

## Selection and de-duplication

Before selecting this batch, the available tracking in `dheitmueller/wireshark-chatgpt` was enumerated and consulted, including `reviewed-mrs.md`, the aggregate automation ledger, and all per-run files under `reviewed-mrs-automation/`. Selection was based on represented MR numbers rather than assuming that every number inside a ledger filename range was necessarily reviewed. The separately tracked !25759-!25827 work remains accounted for through the individual tracking, rather than being inferred from a range filename.

The historical reviewed batch **!17571-!17620** remains part of the already-reviewed set and was explicitly preserved and counted through `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md`.

The immediately preceding descending batch ends at !23259. After checking the review tracking for higher-numbered holes, the fifty highest-numbered corpus MRs not already represented were confirmed to be **!23258 through !23209 inclusive**.

## Exact MRs reviewed

1. !23258
2. !23257
3. !23256
4. !23255
5. !23254
6. !23253
7. !23252
8. !23251
9. !23250
10. !23249
11. !23248
12. !23247
13. !23246
14. !23245
15. !23244
16. !23243
17. !23242
18. !23241
19. !23240
20. !23239
21. !23238
22. !23237
23. !23236
24. !23235
25. !23234
26. !23233
27. !23232
28. !23231
29. !23230
30. !23229
31. !23228
32. !23227
33. !23226
34. !23225
35. !23224
36. !23223
37. !23222
38. !23221
39. !23220
40. !23219
41. !23218
42. !23217
43. !23216
44. !23215
45. !23214
46. !23213
47. !23212
48. !23211
49. !23210
50. !23209

## Review notes

| MR | Outcome / review depth | Durable evidence |
| --- | --- | --- |
| !23258 | Scanned, merged | `ptvcursor` unsigned offset migration; reinforces existing unsigned-offset/API-domain guidance. |
| !23257 | Scanned, merged | Propagates SDP-discovered RFCOMM service UUID context to later packets and provides a sample capture; useful stateful dissector feature, no new general rule. |
| !23256 | Discussion/diff reviewed, merged | RESP Map/Attribute support split from a larger MR at reviewer request. Review focused mainly on contribution metadata; scope splitting is consistent with existing submission practice. |
| !23255 | Scanned, merged | Corrects a previous Kafka UB fix after discovering that `-1` remains a meaningful count sentinel in parts of the protocol implementation; strong warning against semantic regressions in analyzer-driven cleanup. |
| !23254 | Scanned, merged | Gerald Combs adds explicit Stratoshark test enablement and coverage; focused test-harness maintenance. |
| !23253 | Scanned, merged | New find API and unsigned offsets across several dissectors; existing API migration guidance. |
| !23252 | Scanned, merged | John Thacker fixes an ineffective comparison caused by unsigned subtraction; existing guard-before-subtract/arithmetic-domain guidance. |
| !23251 | Scanned, merged | Updates Mobile IPv6 identifier registry handling; correctly uses `FT_BYTES` for non-string identifiers and also fixes an offset overflow. Good semantic field-typing evidence, but not promoted as a separate rule in this batch. |
| !23250 | Scanned, merged | BER-TLV offset handling simplification/fix; parser-offset cleanup consistent with existing parsing guidance. |
| !23249 | Scanned, merged | Makes an unreachable switch state explicit for Coverity where the helper contract admits only two values; existing assertion/static-analysis guidance. |
| !23248 | Scanned, merged | DHCP MS Option 77 concatenation follows RFC 3396 semantics; binary user-class data is represented as `FT_BYTES` rather than pretending all data is text. |
| !23247 | Scanned, merged | Straightforward corrections identified by `check_typed_item_calls.py`; no independent new convention. |
| !23246 | Scanned, merged | Repairs incorrect earlier changes made in response to false positives from `check_typed_item_calls.py`; strongly corroborates existing guidance to fix analyzer models rather than distort valid code. |
| !23245 | Scanned, merged | Extends `check_typed_item_calls.py` to understand legal BOM/VARINT encodings after false warnings caused incorrect commits. Reinforces the same analyzer-model rule. |
| !23244 | Scanned, merged | Python 3.14's POSIX default moving from `fork` to `forkserver` requires multiprocessing startup under `if __name__ == '__main__'`; corroborates the already-recorded spawn/forkserver portability rule. |
| !23243 | Scanned, merged | Adds integer and double preference types; focused preference API extension. |
| !23242 | Scanned, merged backport | Guy Harris-authored release-4.6 backport allowing spec-valid zero-length normal CAN frames in TTL. Strong corroboration of merged master !23240. |
| !23241 | Scanned, closed | Earlier release-4.6 backport attempt of the TTL zero-length fix; down-weighted in favor of merged !23242. |
| !23240 | Scanned, merged | Master TTL fix: zero-length normal CAN frames are valid by specification and must not cause the remainder of the block to be discarded as corrupt. |
| !23239 | Scanned, merged backport | Guy Harris release-4.4 backport of dumpcap man-page corrections; documentation maintenance. |
| !23238 | Scanned, closed | Backport attempt for multi-commit `validate-commit.py` support; down-weighted because it did not merge. |
| !23237 | Scanned, merged | Kafka OSS-Fuzz overflow UB fix plus null compact-array handling; existing arithmetic/sentinel guidance. |
| !23236 | Scanned, merged | Keeps Homebrew falco-libs version in sync with the version installed elsewhere; dependency maintenance. |
| !23235 | Scanned, merged | Corrects ineffective unsigned checks in CIP Motion after Coverity findings; existing arithmetic-domain guidance. |
| !23234 | Scanned, merged | Corrects another ineffective unsigned overflow check in ANSI A; existing arithmetic-domain guidance. |
| !23233 | Scanned, merged | RTPDump migration to newer find API and unsigned offsets; existing migration guidance. |
| !23232 | Scanned, merged | Gerald Combs makes protocol names easier to locate with repository grep; maintainability cleanup. |
| !23231 | Scanned, merged | `tvb_get_ptr()` switches to unsigned offsets/lengths; importantly moves the fetch until after `-1`/remaining-length normalization. Strong corroboration of validate/normalize-before-fetch guidance. |
| !23230 | Scanned, merged | Correct Qt plural forms for RTP stream counts; localization/UI correctness. |
| !23229 | Scanned, merged backport | release-4.4 backport of current TLS 1.3 SignatureScheme registry values. |
| !23228 | Scanned, merged backport | release-4.6 backport of current TLS 1.3 SignatureScheme registry values. |
| !23227 | Scanned, merged | Gerald Combs ensures the repository `.gitmessage` template remains LF-terminated; developer-workflow portability maintenance. |
| !23226 | Deep discussion/diff review, merged | Gerald Combs fixes Stratoshark startup failures when shared plugins register against dissector tables/capabilities absent in that frontend. Promoted to `frontend-registration-conventions.md`. |
| !23225 | Scanned, merged | Anders Broman conversion to `_expert_remaining` helpers and unsigned offsets; existing bounds/offset guidance. |
| !23224 | Scanned, merged | Master update of TLS 1.3 SignatureScheme registry, with !23228/!23229 as stable backports. |
| !23223 | Scanned, closed | Failed/superseded release-4.4 backport attempt for multi-commit validation support; down-weighted. |
| !23222 | Scanned, closed | Earlier failed/superseded backport attempt for the same validator change; down-weighted. |
| !23221 | Scanned, closed | Superseded dumpcap man-page backport attempt; down-weighted in favor of merged !23239. |
| !23220 | Deep semantic review, merged | Fixes stats-tree floating maximum initialization: `FLT_MIN` is the smallest positive normalized float, not a negative extremum. Promoted to `arithmetic-safety-conventions.md`. |
| !23219 | Scanned, closed | Earlier dumpcap man-page backport attempt; down-weighted in favor of merged !23239. |
| !23218 | Scanned, merged backport | release-4.6 backport of dumpcap man-page corrections. |
| !23217 | Scanned, merged | Guy Harris-authored master dumpcap man-page correction; documentation-only. |
| !23216 | Scanned, merged | Gerald Combs removes obsolete macOS setup patches after the script stopped building the affected libraries; remove dead build workarounds when their premise disappears. |
| !23215 | Discussion-aware, still open in corpus snapshot | Follow-on profile/model restructuring. It is intentionally down-weighted relative to merged !23210 because its design was not yet accepted at the corpus commit. |
| !23214 | Scanned, merged | release-4.4 version bump; release engineering only. |
| !23213 | Scanned, merged | release-4.6 version bump; release engineering only. |
| !23212 | Scanned, merged | release-4.4 build marker; release engineering only. |
| !23211 | Scanned, merged | release-4.6 build marker; release engineering only. |
| !23210 | Deep discussion/diff review, merged | Large Profile Dialog/ProfileModel refactor with extensive Stig Bjørlykke scenario testing. Reviewer feedback established that external profile/extcap installers must be visible without restart and that filesystem discovery remains authoritative; the merged design also stages profile edits until acceptance. Promoted to `profile-conventions.md`. |
| !23209 | Scanned, merged | Michael Mann fixes a language-preference crash caused by double-freeing memory that only appeared to leak; reinforces existing ownership/lifetime review. |

## Durable findings promoted to the notebook

### !23210 — Profile discovery must remain externally observable, and profile edits are transactional

During the long review of the eventually merged Profile Dialog / ProfileModel refactor, Stig Bjørlykke found that an early form of profile caching failed to notice profile directories created while Wireshark was running. He identified a concrete supported use case: an external installer may add an extcap binary with a corresponding profile, and users must not have to restart Wireshark before that profile appears. He explicitly required directory discovery, rather than a process-lifetime cached list, to remain authoritative.

The same merged refactor keeps dialog edits as staged model state and writes them only when the user accepts the dialog. Reviewer testing exercised duplicate names, delete/rename interactions, multiple imports, externally modified profiles, and cancellation/validation behavior before merge.

Promoted to `profile-conventions.md`.

### !23226 — Shared registration code must handle frontend-specific dissector capabilities

Merged !23226, authored and merged by Gerald Combs, was reproduced with Stratoshark on Windows when Wireshark and Stratoshark plugins occupied the same build directory. Wireshark-oriented plugins attempted to register range preferences and Decode-As behavior against tables such as `udp.port` and `ethertype` that did not exist in the Stratoshark registration environment. The accepted change checks that the table exists and that Decode-As is supported before performing those operations, turning an unsafe startup assumption into a validated runtime capability.

Promoted to `frontend-registration-conventions.md`.

### !23220 — `FLT_MIN` is not the floating-point analogue of `INT_MIN`

Merged !23220 corrects a stats-tree maximum accumulator initialized with `FLT_MIN`. Because `FLT_MIN` is the smallest positive normalized `float`, a data set consisting entirely of negative values never updates the maximum correctly. The accepted fix uses `-FLT_MAX` as the negative finite sentinel.

Promoted to `arithmetic-safety-conventions.md`.

## Strong corroborating/contextual evidence not duplicated as new rules

- **!23245 and !23246**, together with **!23255**, form a strong static-analysis lesson: false-positive checker output caused semantically incorrect edits, and later accepted changes repaired both the checker and those edits. `assertion-static-analysis-conventions.md` already says to improve analyzer models rather than distort established correct code, so this batch records the corroboration without duplicating the rule.
- **!23244** independently corroborates the existing Python multiprocessing portability guidance: code must work when `fork` inheritance is unavailable, including Python 3.14's POSIX `forkserver` default and Windows/macOS spawn-like behavior.
- **!23258, !23253, !23233, !23231, and !23225** continue the accepted unsigned-offset/find/remaining-length migration and are covered by existing TVB/type-domain guidance.
- **!23252, !23237, !23235, and !23234** reinforce existing unsigned-arithmetic and intermediate-expression safety rules.
- **!23242** gives high-authority Guy Harris backport evidence for the master TTL fix in **!23240**. The closed !23241 attempt was not treated as equivalent evidence.
- **!23239/!23218/!23217** are the accepted dumpcap documentation sequence; closed !23219/!23221 were explicitly down-weighted as superseded attempts.
- **!23224/!23228/!23229** are a master-plus-stable-backport sequence for current TLS SignatureScheme registry data and primarily demonstrate normal maintenance/backport practice.
- **!23209** reinforces ownership review: what looks like a leak may actually be memory already owned/freed elsewhere, so adding cleanup without proving ownership can create a double free.

## Evidence weighting

Merged master MRs and accepted stable backports were given substantially more weight than closed or still-open work. In particular, closed !23219/!23221/!23222/!23223/!23238/!23241 were treated as superseded or unsuccessful attempts, while open !23215 was retained only as provisional context. Guy Harris-authored/merged evidence in !23217, !23218, !23239, and !23242 was weighted highly, but documentation/backport-only changes were not inflated into architectural rules merely because of author authority.

The strongest reusable evidence in the batch came from the extensive maintainer/domain review around !23210, Gerald Combs's reproduced cross-frontend failure and accepted fix in !23226, and the direct numeric-correctness fix in !23220.