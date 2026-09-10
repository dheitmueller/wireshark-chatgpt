# Wireshark MR review batch: !25259–!25308

## Corpus provenance

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit used for this entire review batch: **`9e52bc78659a888d4eb624984ee1a886a40d959f`**
- Exactly fifty previously unreviewed MRs were reviewed: **!25259 through !25308 inclusive**.
- Selection was made by rebuilding the already-reviewed set from `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all per-run files in `reviewed-mrs-automation/`. The previously reviewed **!17571–!17620** batch was explicitly preserved and counted. No numeric range was assumed reviewed solely because another entry in that range appeared in a ledger.
- Review direction remains newest-to-oldest. These were the fifty highest-numbered corpus MRs absent from the rebuilt reviewed set at selection time.
- Merged master MRs were weighted most heavily; release backports were generally corroboration; closed/unmerged work was treated as lower-confidence evidence.

## Exact reviewed set

| MR | Review | Notes |
|---|---|---|
| !25308 | Scanned | release-4.6 backport of the Qt translation-warning refinement from !25302. Merged; no additional durable lesson. |
| !25307 | Scanned | Qt/CMake compatibility fix uses the Qt 6.2 `qt_add_lupdate` signature so Qt < 6.7 remains buildable; Martin Mathieson confirmed it on Qt 6.4.2. Useful compatibility corroboration, but no new notebook rule. |
| !25306 | Scanned | release-4.4 backport of the eDonkey monotonic-offset and BLOB-width fix from !25304. Corroboration only. |
| !25305 | Scanned | release-4.6 backport of !25304. Corroboration only. |
| !25304 | Deep | John Thacker master fix rejects list-item parsers that fail to advance (`offset <= item_start_offset`) and corrects BLOB field width from 16 to 32 bits. Strong reinforcement of existing monotonic-parser-progress and field-width consistency guidance. |
| !25303 | Scanned | CMake CpuInfo wording/URL/self-build documentation cleanup. Merged; no reusable engineering convention. |
| !25302 | Scanned | Qt translation warning now identifies the translation catalog/search path and suppresses the expected untranslated C locale. Useful diagnostics cleanup, but no broader rule beyond existing diagnostic guidance. |
| !25301 | Scanned | release backport of UTF-8-safe truncation handling for long IEEE manufacturer names. Corroborates existing string/truncation safety guidance. |
| !25300 | Scanned | second release backport of the IEEE manufacturer UTF-8 truncation fix. No additional lesson. |
| !25299 | Scanned | Moves Qt translation generation to newer Qt CMake commands while retaining the supported Qt baseline. Build-system maintenance; no distinct durable rule. |
| !25298 | Scanned | Master IEEE manufacturer-name truncation fix: reacts to `g_strlcpy` truncation and preserves valid UTF-8 rather than blindly ending in a partial multibyte sequence. Reinforces existing UTF-8/output safety guidance. |
| !25297 | Scanned | Release backport of the GitLab CI migration from `mc` to `rclone`. No broader lesson. |
| !25296 | Scanned | dumpcap comment indentation cleanup. No reusable lesson. |
| !25295 | Scanned | GitLab CI migration of additional Ubuntu jobs from `mc` to `rclone`. Tooling-specific migration; no durable general rule extracted. |
| !25294 | Scanned | dumpcap determines application flavor during the early argument-parsing layer so flavor-dependent behavior is known before first use. Consistent with existing initialization/order guidance. |
| !25293 | Scanned | Qt Lua debugger changes eliminate `lupdate` translation warnings and make translatable context explicit. Localization-specific cleanup. |
| !25292 | Scanned | Qt recent-filter arrow gains proper hover/pressed themed-icon states. UI polish; no durable architectural lesson. |
| !25291 | Scanned | Release packaging backport installs required OpenCORE AMR DLLs under MSYS2. Packaging-specific. |
| !25290 | Scanned | Qt translation loading honors the OS preferred UI-language ordering on Windows rather than assuming one locale name. Platform-localization fix; no separate general rule needed. |
| !25289 | Scanned | “no-options” CI explicitly disables `ENABLE_ZLIBNG` because it defaults independently of `ENABLE_ZLIB`. Reinforces that negative/minimal build configurations must disable independently defaulted features explicitly. |
| !25288 | Scanned | Avoids installing Qt translation files a second time on Windows when they are already embedded as resources. Packaging/resource cleanup. |
| !25287 | Scanned | Stratoshark no-libpcap build guards the whole capture-restart connection so an unavailable action cannot leave an invalid capture/use path. Build-configuration correctness; existing optional-feature guidance suffices. |
| !25286 | Scanned | Introduces `MainApplication::whenInitialized()` to replace repeated “already initialized vs connect for later” call-site logic. Reinforces centralized lifecycle gating. |
| !25285 | Deep | Large merged Qt refactor makes `InterfaceListManager` the single owner/refresh source for interface enumeration and statistics, moves sampling off-thread, coalesces/defer refreshes during capture, and makes multiple GUI consumers use the same statistics. Strong historical corroboration of the later, already-recorded !26260/!26262/!26265/!26270/!26274 authoritative-owner architecture; not duplicated. |
| !25284 | Scanned | Automatic data/translation update. No durable lesson. |
| !25283 | Scanned | Automatic data/translation update. No durable lesson. |
| !25282 | Scanned | Automatic data/translation update. No durable lesson. |
| !25281 | Scanned | Release backport of missing D28 offset advancement. Straight parser-cursor correction; no additional convention. |
| !25280 | Scanned | MinGW tooling installs Qt default translations required for deployment. Packaging-specific. |
| !25279 | Scanned | Master MSYS2 packaging fix installs OpenCORE AMR codec DLLs required at runtime. Packaging-specific. |
| !25278 | Scanned | Moves application-flavor selection for dumpcap pipe arguments down to the layer that owns argument construction rather than passing redundant state through higher layers. Useful ownership cleanup, but not sufficiently broad to add a new rule. |
| !25277 | Scanned | 3GPP XML nettrace reader populates the wiretap file start timestamp from parsed `beginTime`. File-metadata correctness fix. |
| !25276 | Scanned | Corrects `beginTime` lookup to the XML element that actually owns the attribute. Straight schema/parser correction. |
| !25275 | Scanned | Master fix advances parser offset when D28 is present. Reinforces existing parser-progress/accounting rules. |
| !25274 | Discussion-focused (closed) | Proposed forcing the Windows Vista Qt style to restore an older UI feel. Closed/unmerged, so treated as low-weight context rather than accepted design. |
| !25273 | Scanned | Release backport of Windows preferred-UI-language handling. Corroboration only. |
| !25272 | Scanned | Aligns `proto_set_appendix_len` start/length types with their already-unsigned field-information domain. Reinforces existing semantic integer-domain guidance. |
| !25271 | Deep | John Thacker fixes UBSan-reported undefined behavior where a generic `QObject` parent was `qobject_cast` to `QWidget` and dereferenced despite possible null. Accepted code obtains the real `MainWindow`, checks it, then constructs the dialog. Promoted to `qt-object-parenting-conventions.md`. |
| !25270 | Scanned | Empty display-filter application emits the semantic `cleared` signal rather than silently doing nothing. UI state-transition correctness; no separate rule extracted. |
| !25269 | Scanned | Funnel menu teardown frees callback data at the owning lifecycle point while avoiding a second free during reload. Reinforces existing ownership/lifetime guidance. |
| !25268 | Scanned | Bluetooth HCI adds Core Spec 6.1–6.3 command/event support. Protocol feature expansion; no reusable review correction found. |
| !25267 | Scanned | Corresponding Bluetooth HCI spec-support backport/companion change. No additional lesson. |
| !25266 | Scanned | Removes unused Qt source files. Maintenance only. |
| !25265 | Scanned | Makes color-scheme preference global rather than profile-scoped to match its product semantics. Reinforces putting configuration at the lifetime/scope where the behavior actually belongs. |
| !25264 | Scanned | Treats an empty capture filter as semantically valid (“capture everything”) rather than rejecting it because the editor's generic status enum lacked that case. UI/protocol semantics correction. |
| !25263 | Deep | John Thacker fixes `FT_PROTOCOL` backing-tvbuff semantics: rebase at the field start but retain the remaining data source so later `proto_item_set_len`/`finfo_set_len` growth remains valid, then re-enable backing-length assertions. Promoted to `protocol-field-backing-conventions.md`. |
| !25262 | Scanned | Funnel teardown nulls the GUI operations pointer and consumers check it after GUI deletion, preventing stale-use/leak paths. Reinforces lifetime-state invalidation guidance. |
| !25261 | Scanned | Funnel GUI operation hooks are installed before `epan_init()` because Lua plugin registration occurs during that initialization. Initialization-order fix; no additional rule beyond existing lifecycle guidance. |
| !25260 | Scanned | Makes SpeexDSP conditional on the Qt Multimedia paths that actually use it. Optional-dependency cleanup; existing build-configuration guidance suffices. |
| !25259 | Deep | Martin Mathieson extends `check_typed_item_calls.py` to detect duplicate hand-written `value_string` definitions. Discussion distinguishes the manageable hand-written set from nearly two million generated occurrences, where generator-level handling is more appropriate. Promoted to `ci-tooling-conventions.md`. |

## Notebook updates promoted from this batch

- `qt-object-parenting-conventions.md`: generic `QObject` parentage must not be assumed to satisfy a `QWidget` ownership requirement; validate casts/nullness and prefer the semantically guaranteed widget owner (!25271).
- `protocol-field-backing-conventions.md`: mutable protocol-field lengths require a backing tvbuff with the correct start origin and sufficient remaining extent for legal growth; establish the invariant before enabling assertions (!25263).
- `ci-tooling-conventions.md`: encode deterministic, low-noise structural conventions in existing repository checkers, while handling generated code at generator/tooling scope rather than flooding source checks (!25259).

Several other MRs in this batch strongly reinforced rules already present—especially !25304–!25306 parser progress, !25285 authoritative GUI state ownership, !25272 semantic unsigned domains, !25269 ownership/lifetime, and !25298–!25301 UTF-8-safe truncation—so those lessons were deliberately not duplicated in the core notebook.
