# Automated Wireshark MR review: !24659-!24708

## Corpus provenance

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit used for this entire review batch: **`9e52bc78659a888d4eb624984ee1a886a40d959f`**
- Notebook base before this run: **`d08cd5d26a87cf6a5d973f4cb6e999068a269107`**
- Exactly fifty previously unreviewed MRs were reviewed: **!24659 through !24708 inclusive**.
- Selection was made after consulting `reviewed-mrs.md`, the aggregate automation tracking, and the available per-run ledgers. Individual MR membership, not broad numeric-range assumptions, was used to build the already-reviewed set.
- The previously reviewed historical batch **!17571-!17620** remains preserved and counted in the reviewed set.

## Exact reviewed set

| MR | Review | Notes |
|---|---|---|
| !24708 | Scanned | Merged John Thacker tooling fix avoids concurrent `pngcrush` temporary-name collisions caused by assuming shell `$$` expansion inside Python. Useful concurrency cleanup; no distinct notebook rule promoted. |
| !24707 | Scanned | Merged release-4.6 sharkd man-page backport. Documentation/backport maintenance only. |
| !24706 | Scanned | Merged release-4.4 VeriWave defensive fix changes signature search to return success/failure rather than exposing an invalid offset to callers. Corroborates safer parser API contracts. |
| !24705 | Deep/corroboration | Merged John Thacker release-4.6 CI change runs Qt 5 coverage for Qt changes because master no longer exercises the same compatibility surface. Reinforces the existing stable-branch CI matrix convention. |
| !24704 | Scanned | Merged Windows packaging backport excluding `icuuc.dll`; corroborates the already-recorded build-host DLL portability rule. |
| !24703 | Scanned | Merged release-4.6 JSON wiretap fix rejects degenerate zero-token inputs instead of falsely identifying them as JSON. Backport corroboration. |
| !24702 | Scanned | Merged release-4.6 VeriWave signature-search fix. Same lesson as !24706/!24700. |
| !24701 | Scanned | Merged dissector cleanup uses combined tree/value helper paths instead of separately fetching values. Fits existing typed-item/helper guidance. |
| !24700 | Scanned | Merged John Thacker master VeriWave fix makes signature detection a boolean contract, preventing invalid-offset use. No separate rule beyond established parser API safety. |
| !24699 | Discussion/diff | Merged John Thacker JSON wiretap format-detection hardening rejects whitespace/NUL-like inputs with no parsed tokens. Strong graceful-detection example; existing input-validation guidance is sufficient. |
| !24698 | Scanned | Merged master Windows packaging fix excluding build-host `icuuc.dll`; already captured by prior portability convention. |
| !24697 | Scanned | Merged WiX sharkd man-page packaging change. No general convention. |
| !24696 | Scanned | Merged Procmon UTF-16 conversion fix uses fallback-aware conversion with the correct source length. Corroborates robust text-conversion guidance. |
| !24695 | Scanned/lower independent weight | Merged AI-assisted Lua debugger breakpoint enhancement, self-authored/self-merged by Stig Bjørlykke with no review thread. Useful feature work but insufficient independent evidence for a new broad rule. |
| !24694 | Scanned | Merged setup-script change enables PNG optimization utilities. Tooling maintenance. |
| !24693 | Deep | Merged John Thacker CI change introduces PNG-compressibility checking as advisory rather than gating because contributor remediation tools may be unavailable; development also exposed an initially inert CI environment missing those tools. Promoted to `ci-hygiene-conventions.md`. |
| !24692 | Scanned | Merged sharkd HTML-page installation correction. Packaging-specific maintenance. |
| !24691 | Scanned | Merged John Thacker BER length arithmetic hardening supports large objects without overflow. Reinforces existing arithmetic-safety conventions. |
| !24690 | Scanned | Merged stable-branch macOS deployment-target update. Platform maintenance only. |
| !24689 | Deep | Merged ENIP feature with substantive Jaap Keuter review: replace legacy GLib integer aliases with `uint8_t`, use combined value/tree helpers, remove incorrect `_U_`, and follow existing minimum-length checks. The type rule was promoted to `c-type-conventions.md`; remaining points reinforce existing dissector guidance. |
| !24688 | Deep | Merged PROFINET SXP refactor moves PNIO block decoding out of the SXP-specific proof-of-concept path and into the common PNIO implementation for SXP and non-SXP traffic. Anders Broman approved. Promoted to `capture-decoder-boundary-conventions.md`. |
| !24687 | Scanned | Merged formatting/toolchain fix replaces a problematic `snprintf` path with `strftime`. Narrow compiler-diagnostic cleanup. |
| !24686 | Scanned | Merged signed-overflow hardening in Peekclassic parsing. Reinforces arithmetic-safety guidance. |
| !24685 | Scanned | Merged O-RAN filter enhancement. Feature-specific behavior; no durable convention extracted. |
| !24684 | Scanned | Merged Lua debugger UI control change. Presentation-specific. |
| !24683 | Scanned/lower independent weight | Merged AI-assisted Lua debugger watch-expression feature, self-authored/self-merged with no review comments. Its bounded evaluation design is sensible but not promoted without stronger independent review evidence. |
| !24682 | Scanned | Merged wmem API efficiency/safety cleanup returns newly inserted frames directly rather than performing a second nullable search. Useful API improvement; no new Wireshark-wide rule. |
| !24681 | Scanned | Merged release-4.4 version bump. No engineering lesson. |
| !24680 | Scanned | Merged DHCP parser cleanup uses `tvb_get_string_uint` instead of allocation/copy/termination plus conversion. Reinforces use of purpose-built TVB helpers. |
| !24679 | Scanned | Merged initial sharkd manual-page work. Documentation-focused; no durable coding/architecture rule. |
| !24678 | Scanned | Merged release-4.6 version bump. No engineering lesson. |
| !24677 | Scanned | Merged John Thacker E.212 parser cleanup uses `tvb_get_string_uint16` and expert diagnostics on conversion failure. Reinforces purpose-built parser helpers plus diagnostics. |
| !24676 | Deep/corroboration | Merged John Thacker OSS-Fuzz fix uses `tvb_captured_length_remaining` instead of offset-existence arithmetic to reduce overflow risk. Strong corroboration for existing boundary-helper guidance. |
| !24675 | Deep | Merged John Thacker fuzzshark fix moves shared `app_data` initialization outside an aggregate-only `#ifdef` so individual targets start from the same zeroed baseline. Promoted to `fuzz-harness-conventions.md`. |
| !24674 | Scanned | Merged release-4.4 build/version update. No durable lesson. |
| !24673 | Scanned | Merged release-4.6 build/version update. No durable lesson. |
| !24672 | Scanned | Merged John Thacker Coverity configuration excludes third-party build-time Lemon source from low-value analysis noise. Sensible static-analysis scoping; no separate rule promoted. |
| !24671 | Scanned | Merged release-4.6 sponsor-slide manual backport after automation failure. Project-content maintenance. |
| !24670 | Scanned | Merged John Thacker SIP follow-up fixes offset-vs-length and reversed-condition mistakes introduced during earlier API conversions. Reinforces semantic verification after mechanical API migration. |
| !24669 | Scanned | Merged release-4.4 release-preparation change. No engineering lesson. |
| !24668 | Scanned | Merged release-4.6 release-preparation change. No engineering lesson. |
| !24667 | Scanned | Merged sponsor-slide update on master. Project-content maintenance. |
| !24666 | Deep | Merged ETW architecture refactor preserves raw properties/extended data in the capture path and moves semantic user-message formatting into the parser so fields remain independently visible/filterable. Anders Broman approved. Promoted to `capture-decoder-boundary-conventions.md`. |
| !24665 | Scanned | Merged John Thacker androiddump change logs unlikely `fcntl` failures found by Coverity. Diagnostic maintenance. |
| !24664 | Scanned | Merged John Thacker cleanup removes a pointless NULL check after earlier unconditional dereferences establish the invariant. No new convention. |
| !24663 | Deep/corroboration | Merged Stig Bjørlykke/John Thacker Coverity fix locks debugger shared-state reads, snapshots state under the mutex, and releases the lock before nested Lua/UI work to avoid deadlock. Strong concurrency example; existing lifetime/reentrancy guidance is sufficient. |
| !24662 | Deep | Merged first-contribution OAMPDU feature. Jaap Keuter explicitly required new feature intake on master rather than a stable release branch and clarified that retargeting is not a substitute for rebasing/recreating against the correct base. Promoted to `stable-branch-submission-conventions.md`. |
| !24661 | Scanned | Merged dissector-script warning cleanup. Mechanical/static-check maintenance. |
| !24660 | Scanned | Merged release-4.4 RTP-MIDI quarter-frame offset fix. Stable backport of a focused dissector bug fix. |
| !24659 | Scanned | Merged release-4.6 RTP-MIDI quarter-frame offset fix. Stable backport; no additional convention. |

## Durable notebook changes from this batch

1. **New features target master; stable branches remain stabilization/backport lines.** !24662 also establishes that changing the target branch does not replace rebasing/recreating against the intended base.
2. **Preserve raw structured acquisition data and perform semantic formatting in the decoder when it improves filterability and reuse.** !24666 moves ETW formatting from etwdump into the parser.
3. **Put shared wire-format decoding in the canonical protocol implementation.** !24688 replaces SXP-local PNIO block parsing with shared PNIO decoding.
4. **Allow nonessential hygiene checks to start advisory when remediation tooling is not broadly available, while ensuring the CI environment actually executes the check.** !24693.
5. **Initialize shared fuzz-harness state across every specialized and aggregate target configuration.** !24675.
6. **Prefer standard C fixed-width integer types in Wireshark-owned dissector code, while preserving library-defined types at real external API boundaries.** Jaap Keuter's review of merged !24689.

Strong items retained as corroboration rather than duplicated include !24705 for stable-branch CI coverage, !24691/!24686/!24676 for arithmetic and bounds safety, !24663 for lock scope around nested callbacks, and !24699/!24703 for conservative wiretap format detection.
