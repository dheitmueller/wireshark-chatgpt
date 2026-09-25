# Review findings: Wireshark MRs 9412-9461

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

This run reviewed 50 previously unreviewed MRs, 9412 through 9461. Forty-nine were merged. MR 9417 was closed without merge and is therefore down-weighted. Merged master changes and direct maintainer guidance are treated as stronger evidence than backports or superseded work.

## Durable findings promoted to notebook conventions

### MR 9452: whole-capture analyses need read exclusion

John Thacker's merged Protocol Hierarchy Statistics fix acquires the capture-file read lock, refuses to start while another read or rescan is active, uses the capture file's shared stop flag, and disables the UI action during conflicting work. This prevents file-close/read races and progress-dialog ownership problems. Promoted to `capture-traversal-conventions.md`.

### MRs 9438, 9447, 9437, 9449: Qt model values need semantic types

John Thacker's merged sorting and filtering fixes use typed model data for comparison rather than formatted display strings. MR 9438 also documents that QVariant type convertibility is not proof that a particular string value converts successfully. MR 9447 preserves visible percentage rounding while comparing typed values. MR 9437 and its backport 9449 reinforce that displayed column positions are not stable source-model identities when optional columns are hidden. Promoted to `qt-model-value-conventions.md`.

### MR 9436: consume C predicate results as zero versus nonzero

Guy Harris pointed out that the C standard does not require `isinf` to return exactly 1 or -1; it only guarantees zero versus nonzero. The merged cleanup changed exact-value tests to a direct predicate test. This is strong standards-based maintainer evidence and is promoted to `c-predicate-conventions.md`.

### MRs 9418 and 9451: generated derivatives follow their owning workflow

Pascal Quantin redirected an AUTHORS correction to `.mailmap` because AUTHORS is generated. In MR 9451, Jaap Keuter asked that a conflicted generated enterprise TSV be dropped because the normal automatic update would regenerate it. The durable distinction is that not all generated files have the same submission workflow: when a scheduled project job owns a derivative, change the authoritative source or generator and let that job update the derivative. Promoted to `generated-derivative-workflow-conventions.md`.

### MRs 9415 and 9416: foundational state must survive optional analysis preferences

TCP base-sequence initialization was hidden inside the optional sequence-analysis path even though it was also required for stream/conversation reuse detection. Moving the state update outside the preference gate fixes stream numbering when sequence analysis is disabled. Promoted to `dissector-preference-state-conventions.md`.

### MRs 9455-9461: distinguish widget defaults, refresh, and styling contracts

MR 9458 initializes an Expert Information checkbox from the display filter only when the dialog opens; normal refreshes no longer overwrite the user's later choice. The ElidedLabel fixes use the widget's semantic small-text method instead of HTML-like markup that the widget escapes. Promoted to `qt-widget-state-conventions.md`.

## Important corroboration and historical evidence

MR 9414 is useful release-branch evidence for the nested-frame dependency work already captured elsewhere in the notebook. A real capture exposed excessive recursion and severe performance problems; John Thacker explicitly said the backport should include the later 9538 and 9622 fixes as a unit or be closed. This strongly corroborates the existing frame-dependency set/hash and transitive-dependency guidance rather than creating a new rule.

MR 9429 adds Follow WebSocket Stream and correctly taps the unmasked/reassembled WebSocket payload. Its fixed enum and central Qt plumbing are historical rather than current architecture because the later merged MR 9957 replaced the fixed frontend list with registered followers identified by protocol ID.

MR 9423 is moderate field-semantics evidence: a one-bit MP4 field was kept as an integer category with Movie/Index values rather than forced into a Boolean representation. Bit width alone does not determine semantic field type.

MR 9432 shows Alexis La Goutte asking that related Short SSID fields be audited when fixing byte order, and also enforcing the commit-message line-length convention. MR 9431 fixed uninitialized state but immediately exposed a supported old-GCC warning later corrected by MR 9529, reinforcing supported-build baseline testing.

MRs 9448, 9453, and 9454 pass parsed LwM2M object/resource identifiers directly through helper layers instead of reconstructing a URI string and reparsing it. This is good local design evidence for preserving semantic values across helper boundaries.

## Per-MR disposition

9461 merged, release-4.0 backport of the ElidedLabel contract fix. 9460 merged, release-4.0 backport of the Expert Information state fix. 9459 merged master, ElidedLabel semantic styling. 9458 merged master, preserve user checkbox choice after initial default. 9457 merged release-3.6 backport of 9455. 9456 merged release-4.0 backport of 9455. 9455 merged master, use ElidedLabel styling API. 9454 merged release-3.6 LwM2M backport. 9453 merged release-4.0 LwM2M backport. 9452 deep review, promoted capture traversal locking rule.

9451 discussion-focused, promoted generated-derivative workflow rule. 9450 merged release-4.0 backport of typed sorting. 9449 merged release-4.0 column mapping fix. 9448 merged master LwM2M semantic-ID plumbing. 9447 deep review, promoted Qt typed filtering rule. 9446 merged MinGW warning cleanup. 9445 merged release-3.6 compiler-warning guard. 9444 merged release-4.0 compiler-warning guard. 9443 merged release-4.0 donation-link maintenance. 9442 merged release-3.6 AppStream metadata maintenance.

9441 merged release-4.0 AppStream metadata maintenance. 9440 merged master compiler-version warning guard. 9439 merged master donation-link maintenance. 9438 deep review, promoted Qt sort-role and conversion rule. 9437 merged master source/display column mapping fix. 9436 deep review, promoted Guy Harris predicate-return guidance. 9435 merged release-3.6 backport of initialization fix. 9434 merged release-4.0 backport of initialization fix. 9433 merged master AppStream metadata maintenance. 9432 discussion-focused encoding correction and sibling-field audit.

9431 merged master initialization fix with later old-GCC follow-up. 9430 merged release-4.0 OBD-II mode 0x04 correction. 9429 merged master WebSocket follow support; semantic payload handling useful, central enum later superseded. 9428 merged format-string portability cleanup. 9427 merged version-string formatting cleanup. 9426 merged release-4.0 UAT row-dimension backport. 9425 merged BSON Decimal128 raw-byte support. 9424 merged eCPRI/O-RAN capture-driven cleanup. 9423 discussion-focused MP4 segment-box support and categorical field semantics. 9422 merged release-3.6 BGP length backport.

9421 merged release-4.0 BGP length backport. 9420 merged AppImage tooling migration. 9419 merged UAT row-dimension validation fix. 9418 discussion-focused, promoted authoritative `.mailmap` workflow and clean squash. 9417 closed/unmerged nested-dependency backport attempt; down-weighted. 9416 deep review, promoted TCP preference/state separation. 9415 merged release-3.6 backport of 9416. 9414 deep corroboration for nested-dependency correctness and scaling chain. 9413 merged funding metadata maintenance. 9412 merged MSYS2 CI update using the triggering revision for its package recipe; corroborates reproducible CI inputs.

## Review tracking and frontier

The reviewed set was reconstructed from the persistent ledger, supplemental automation tracking, and accumulated per-run exact ledgers rather than from assumed numeric ranges. The historical 17571-17620 batch was rechecked and remains 50 unique reviewed MRs. MR 9411 exists and is merged; only its metadata was used as a frontier probe, so it is not counted as reviewed in this run.

## Notebook files added

`capture-traversal-conventions.md`, `qt-model-value-conventions.md`, `c-predicate-conventions.md`, `generated-derivative-workflow-conventions.md`, `dissector-preference-state-conventions.md`, and `qt-widget-state-conventions.md`.
