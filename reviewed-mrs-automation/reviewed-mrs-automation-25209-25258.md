# Wireshark MR review batch: !25209–!25258

## Corpus provenance

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit used for this entire review batch: **`9e52bc78659a888d4eb624984ee1a886a40d959f`**
- Exactly fifty previously unreviewed MRs were reviewed: **!25209 through !25258 inclusive**.
- Selection was made by rebuilding the already-reviewed set from `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all per-run files in `reviewed-mrs-automation/`. The previously reviewed **!17571–!17620** batch was explicitly preserved and counted. No numeric range was assumed reviewed solely because another entry in that range appeared in a ledger.
- Review direction remains newest-to-oldest. These were the fifty highest-numbered corpus MRs absent from the rebuilt reviewed set at selection time.
- Merged master MRs were weighted most heavily; release backports were generally corroboration; closed/open-draft work was treated as lower-confidence evidence.

## Exact reviewed set

| MR | Review | Notes |
|---|---|---|
| !25258 | Scanned | Merged SpeexDSP optional-dependency backport/companion work; !25253 is the stronger master evidence. |
| !25257 | Scanned | John Thacker temporarily disables another FT_PROTOCOL assertion while the backing-data invariant is repaired; corroborates later !25263. |
| !25256 | Deep | Windows CMake avoids `find_library()` selecting an architecture-mismatched SDK bcrypt artifact and uses the platform linker contract instead. Promoted as target-ABI discovery corroboration. |
| !25255 | Scanned | John Thacker FT_PROTOCOL assertion/comment follow-up; precursor to the stronger accepted !25263 backing-tvbuff fix. |
| !25254 | Scanned | Qt `QMenu` ownership is made explicit because `QPushButton::setMenu()` does not transfer ownership. Reinforces existing Qt ownership guidance. |
| !25253 | Scanned | Master SpeexDSP dependency becomes conditional on the Qt Multimedia consumers that require it. Existing optional-dependency guidance suffices. |
| !25252 | Deep | MDB checksum work; Michael Mann says regression/review pcaps need not be committed and can be attached to the MR for preservation. Useful submission/testing evidence, but not promoted separately this run. |
| !25251 | Deep | Guy Harris improves dumpcap warning reporting for capture-device opens. Strong corroboration of the later capture status/diagnostic contracts already recorded. |
| !25250 | Deep | IPv4 empty-payload check is corrected to compare total length with parsed `hlen`, not a fixed minimum header size. Promoted to parser-boundary guidance. |
| !25249 | Scanned | Guy Harris makes capture-sync formatting/error scratch storage thread-local. Corroborates the later !25662 thread-local-buffer convention already recorded. |
| !25248 | Scanned | Logging/fatal-count maintenance; no distinct durable lesson. |
| !25247 | Scanned | Qt build cleanup removes redundant hand-maintained library macros in favor of package/build-system definitions. Existing single-source/build guidance suffices. |
| !25246 | Deep | WoW derived GUID buffer is zero-initialized before being exposed as a child tvbuff, preventing uninitialized allocator bytes from becoming packet data. Promoted to memory/lifetime guidance. |
| !25245 | Scanned | Replaces an unnecessary Speex-specific integer type with a standard type, reducing header/dependency coupling. No separate rule needed. |
| !25244 | Discussion-focused | MinGW SpeexDSP packaging proposal led toward the optional-dependency architecture accepted in !25253; lower weight than the final master change. |
| !25243 | Scanned | TLS 1.3 PSK early-secret SHA-256 default handling before ServerHello; protocol-specific correctness fix. |
| !25242 | Scanned | Qt font preference UI restricts choices to fonts the application can actually honor rather than silently substituting. UI-specific, not separately promoted. |
| !25241 | Scanned | Qt 6.11 CI/tooling compatibility update; version-specific maintenance. |
| !25240 | Scanned | John Thacker aligns `proto_item_get_len`/related length APIs with the unsigned semantic domain. Reinforces existing API-domain guidance. |
| !25239 | Scanned | eCPRI display improvement showing numeric message type; no broader rule. |
| !25238 | Deep | Known noncompliant HI2 APN/ULI encodings remain decodable, but Anders Broman requests expert information rather than silent normalization. Promoted to protocol diagnostic guidance. |
| !25237 | Scanned | MS-TURN message interpretation uses the protocol discriminator/magic cookie to select semantics. Existing dispatch/heuristic guidance suffices. |
| !25236 | Discussion-focused (closed) | Proposed IPv6 layer/proto-data state scheme exposed recursive/layer complications; closed and therefore retained only as low-weight architectural context. |
| !25235 | Discussion-focused (closed) | Draft Qt5-removal path superseded by accepted follow-up work; not treated as authoritative. |
| !25234 | Scanned | Removes now-obsolete Qt5 compatibility branches after baseline support changed. Maintenance cleanup; no distinct rule. |
| !25233 | Scanned | Qt font preview consistency fix; UI polish only. |
| !25232 | Scanned | BSSMAP LE Release 19 update; Anders Broman requests numerical ordering. Reinforces existing ordering/readability conventions. |
| !25231 | Deep | Guy Harris adds explicit C17/C++17 capability checks on Solaris so configuration fails early with the real prerequisite rather than obscure later probes. Promoted to build conventions. |
| !25230 | Deep | John Thacker corrects a BSSMAP LE two-byte length field that had been parsed as a variable-length quantity; boundary values expose the distinction. Promoted to parser-boundary guidance. |
| !25229 | Scanned | Gerald Combs release-note/version delineation maintenance; release-process specific. |
| !25228 | Scanned | Wireshark 4.7.0 / Stratoshark 0.10.0 build/version bump; mechanical release work. |
| !25227 | Scanned | PROFINET spec update; Anders Broman requests numerical ordering of tables/switch cases. Reinforces existing source-ordering convention. |
| !25226 | Scanned | macOS Qt header sizing preserves native platform baseline while applying zoom delta. Platform-UI behavior, no new cross-cutting rule. |
| !25225 | Scanned | Qt interface-list convenience action; no notable durable review correction. |
| !25224 | Deep | Guy Harris makes Solaris dependency discovery target-architecture aware using the ISA-specific library layout, preventing 32/64-bit package confusion. Promoted to build conventions. |
| !25223 | Scanned | Spelling cleanup; no reusable engineering convention. |
| !25222 | Deep | Theme schema tests had silently self-skipped because CI lacked `jsonschema`; CI now installs the dependency on Linux/Windows. Promoted to CI/tooling guidance. |
| !25221 | Scanned | Qt bookmark font/alignment cleanup; no broader rule. |
| !25220 | Deep | Qt lazy-initialization crash fixed by using the event-filter `watched` popup rather than calling a lazy accessor that recursively constructs another popup. Useful reentrancy evidence; not promoted separately this run. |
| !25219 | Deep | Large Find-in-Packet follow-up incorporates Stig Bjørlykke review on UI consistency, unrelated changes, unused assets, and keeping status-bar presentation logic out of a generic component. Reinforces scope and component-boundary review patterns already present. |
| !25218 | Scanned | release-4.4 backport of asciidoctor-pdf package rename/split. Corroboration only. |
| !25217 | Scanned | release-4.6 backport of asciidoctor-pdf package rename/split. Corroboration only. |
| !25216 | Scanned | Release preparation and documentation updates including Qt5 removal note. No additional convention. |
| !25215 | Deep | John Thacker corrects TLS 1.3 transcript lifetime for PSK client traffic-secret derivation, stopping at Server Finished. Protocol-specific state-boundary correctness. |
| !25214 | Scanned | NAS 5GS ciphered payload becomes a registered FT_BYTES field instead of text-only subtree, making the data filterable. Reinforces field-over-text usability guidance. |
| !25213 | Deep | Guy Harris replaces two coupled request/direction booleans with one request/response/unknown state, clarifying invariants and compiler dataflow. Promoted to state-modeling guidance. |
| !25212 | Scanned | Master CI fix for Ubuntu 26.04 asciidoctor-pdf package split; backports !25217/!25218 corroborate. |
| !25211 | Scanned | Qt margin calculations are centralized so layout and painting share one derived value. Reinforces single-source-of-truth guidance. |
| !25210 | Discussion-focused (open draft) | Contrast-adapting SVG icon proposal remained draft/failed CI; Gerald Combs expressed a preference to standardize on Fluent Icons. Kept as provisional UI direction only. |
| !25209 | Deep | John Thacker removes Qt5 build support after end-of-life and development burden; merged by Anders Broman. Accepted baseline change, with later cleanup separated into follow-up work. |

## Notebook updates promoted from this batch

- `build-conventions.md`: check mandatory compiler/language capabilities early and produce an actionable prerequisite error (!25231); make dependency discovery follow the target ABI rather than the host's default search layout (!25224, corroborated by !25256).
- `ci-tooling-conventions.md`: CI jobs intended to exercise a test family must install its test-only dependencies rather than silently succeeding with those tests skipped (!25222).
- `memory-lifetime-conventions.md`: initialize every byte of a sparsely populated derived buffer before exposing it as packet data/tvbuff storage (!25246).
- `protocol-input-diagnostics-conventions.md`: supporting known real-world noncompliance does not mean silently blessing it; decode when useful and add expert information (!25238, explicit Anders Broman review).
- `state-modeling-conventions.md`: represent one finite semantic state with one enum/value rather than coupled booleans, including explicit unknown state (!25213, Guy Harris).
- `parser-boundary-conventions.md`: use the actual parsed variable header extent for later payload validation (!25250), and verify exact normative field encoding with boundary values rather than inferring from small samples (!25230).

Other notable MRs largely reinforced conventions already present, including capture diagnostic status handling (!25251), reusable thread-local scratch buffers (!25249), Qt ownership (!25254), unsigned semantic domains (!25240), source ordering (!25227/!25232), and MR scope/component separation (!25219). Closed !25235/!25236 and open draft !25210 were explicitly down-weighted.