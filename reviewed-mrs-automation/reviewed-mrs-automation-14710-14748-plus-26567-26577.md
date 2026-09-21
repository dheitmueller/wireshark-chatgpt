# Wireshark MR review automation ledger: !26577-!26567 plus !14748-!14710

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `1c7ad509887ee25079a7865cc62ba18cba06f49a`
Reviewed count: **50**
Status mix: **47 merged, 3 open/unmerged** (`!26577`, `!26574`, `!26570`).

## Selection and duplicate-avoidance

Before selecting this batch, the review state was rebuilt from the notebook's available aggregate tracking (`reviewed-mrs.md` where applicable) and the per-run files in `reviewed-mrs-automation/`. The historical batch **!17571-!17620** remains explicitly preserved and counted. Selection was performed by exact MR-number subtraction rather than assuming that an apparent numeric range was completely reviewed.

The corpus gained previously unreviewed high-numbered entries after the preceding completed run. At this corpus commit, `mr_26567.json` through `mr_26577.json` exist and were not present in the reviewed tracking, while `mr_26578.json` and `mr_26579.json` do not exist. The immediately preceding descending frontier was !14748. Therefore the fifty highest-numbered previously unreviewed MRs were intentionally non-contiguous:

`!26577, !26576, !26575, !26574, !26573, !26572, !26571, !26570, !26569, !26568, !26567, !14748, !14747, !14746, !14745, !14744, !14743, !14742, !14741, !14740, !14739, !14738, !14737, !14736, !14735, !14734, !14733, !14732, !14731, !14730, !14729, !14728, !14727, !14726, !14725, !14724, !14723, !14722, !14721, !14720, !14719, !14718, !14717, !14716, !14715, !14714, !14713, !14712, !14711, !14710`.

## Review notes

- **!26577 — open, provisional/down-weighted.** Adds an SMPTE ST 2038 dissector and MPEG-TS registration-ID plumbing so private stream type `0x06` can be distinguished and passed to ST 2038/ST 291 VANC dissection. Includes a sample and regression tests, but the MR is still open with no maintainer discussion in the corpus snapshot, so no durable convention was promoted from it.
- **!26576 — merged release-4.4, deep.** John Thacker fixes F1AP `UEContextRelease`, where `SRBID` follows the `RRCContainer`. The accepted solution stores the minimum deferred subtree/TVB context in packet-private data and resumes when SRBID arrives. The MR explicitly rejects a dissector-level static `proto_tree *` as a use-after-free anti-pattern because proto trees are freed outside the dissector. Promoted to `protocol-context-state-conventions.md`.
- **!26575 — merged release-4.6, deep/corroborating.** Same accepted F1AP ordering/lifetime fix as !26576 on the other maintained stable branch. Strengthens the promoted packet-owned deferred-context rule.
- **!26574 — open, provisional/down-weighted.** Reworks Linux socket-table enumeration to use `NETLINK_SOCK_DIAG`, with `/proc` fallback and explicit gcc/clang/ASan plus failure-injection testing. Performance rationale is strong, but the MR is unmerged and lacks authoritative review in this snapshot, so it was not used as durable precedent.
- **!26573 — merged.** Automatic 2026-09-20 master data/update change; no substantive review convention.
- **!26572 — merged.** Automatic 2026-09-20 release-4.6 counterpart; no substantive review convention.
- **!26571 — merged.** Automatic 2026-09-20 release-4.4 counterpart; no substantive review convention.
- **!26570 — open, provisional/down-weighted.** Adds Android vendor Bluetooth Quality Report dissection. Pipeline passed, but there is no substantive reviewer discussion and it remains unmerged; no durable convention promoted.
- **!26569 — merged release-4.4, security/correctness.** ETWDump fix keeps a buffer pointer paired with its actual capacity instead of passing an expanded heap-buffer size while still using a stack buffer; it also validates property indexes/sizes. Strong memory-safety corroboration, but existing notebook ownership/bounds guidance already covers the principle.
- **!26568 — merged release-4.6, corroborating.** Stable counterpart of !26569.
- **!26567 — merged release-4.4, deep/corroborating.** Catapult DCT2000 uses the post-truncation effective length for padding and TVB creation and attaches the child TVB to its parent. Strong corroboration of the existing rule to advance/create views using validated or sanitized lengths rather than the original untrusted length.
- **!14748 — merged.** Automatic master update for 2024-03-10; generated/data maintenance only.
- **!14747 — merged.** Automatic release-4.0 update counterpart; maintenance only.
- **!14746 — merged.** Automatic release-3.6 update counterpart; maintenance only.
- **!14745 — merged master, deep.** John Thacker removes duplicate `beginRetapPackets()`/`endRetapPackets()` calls from `TapParameterDialog`. `cf_retap_packets()` already emits Retap Started/Finished events and the inherited `WiresharkDialog` owns those lifecycle callbacks; the duplicate calls caused SRT tables to be added twice. Promoted to `ui-lifecycle-callback-conventions.md`.
- **!14744 — merged.** Additional bounded-recursion checks. Corroborates existing recursion-depth/lint guidance; no duplicate rule added.
- **!14743 — merged release-4.2.** PEP 668/Homebrew CI/tooling adaptation; useful maintenance evidence but not a durable Wireshark architecture rule.
- **!14742 — merged master.** Moves macOS setup toward `pipx` under PEP 668 constraints; build-tooling maintenance, not promoted.
- **!14741 — merged.** Adds GNSS/SBAS message dissectors; protocol feature with no cross-cutting review lesson promoted.
- **!14740 — merged release-3.6.** Recursion checks for BGP/Etch/LI5G/RPL/SIMULCRYPT; corroborates existing bounded-recursion guidance.
- **!14739 — merged release-4.0.** Same recursion-hardening series; corroborating.
- **!14738 — merged release-4.2.** Same recursion-hardening series; corroborating.
- **!14737 — merged release-4.2.** Recursion checks for BFCP/EDONKEY/QNET/RTPS/XMPP; corroborating.
- **!14736 — merged.** Stable-branch recursion-hardening counterpart; corroborating rather than a new rule.
- **!14735 — merged release-4.0.** BFCP/EDONKEY/QNET/RTPS/XMPP recursion-hardening counterpart with balanced depth accounting; corroborates existing guidance.
- **!14734 — merged.** BGP/Etch/LI5G/RPL/SIMULCRYPT recursion-hardening source/master counterpart; corroborating.
- **!14733 — merged master.** Gerald Combs' BFCP/EDONKEY/QNET/RTPS/XMPP recursion hardening, including balanced increment/decrement and targeted lint suppression. Existing notebook coverage is already stronger.
- **!14732 — merged master.** Martin Mathieson changes SANE loop/limit handling to unsigned types matching the 32-bit unsigned wire helper, inspired by Coverity taint warnings. Corroborates exact signedness/domain typing.
- **!14731 — merged.** John Thacker updates WSLua documentation to list `FT_NONE` as a valid payload dissector-table type. Documentation-only corroboration.
- **!14730 — merged master, deep.** John Thacker prevents the TLS debug file and key-log file from resolving to the same filesystem object. It uses `files_identical()`, reports the failure for CLI/GUI, and clears the debug filename before a later path can truncate the key-log file. Promoted with !14728 to `filesystem-file-identity-conventions.md`.
- **!14729 — merged master.** John Thacker uses `BASE_ALLOW_ZERO` for Opus frame data because a zero-length frame is a valid value, not a missing field. Strong corroboration of existing `empty-value-api-conventions.md`.
- **!14728 — merged master, deep.** John Thacker changes Windows `files_identical()` from normalized pathname comparison to volume serial + `FILE_ID_INFO`, correctly handling hard links and avoiding false equality for empty strings. Promoted with !14730 to `filesystem-file-identity-conventions.md`.
- **!14727 — merged master.** Adds a narrowly justified clang-tidy recursion suppression where recursion can happen at most once because the recursive call clears the controlling flag. Corroborates existing rule that a suppression should carry a local proof of bounded recursion.
- **!14726 — merged master.** Fixes Opus multi-frame VBR grammar: only the first N-1 frame sizes are encoded and the final frame consumes the validated remainder after padding. Also exposes frame size as an integer field and tightens checks. Useful parser-boundary corroboration.
- **!14725 — merged.** Zigbee Green Power decryption support for Application ID `0b0010`; no cross-cutting convention promoted.
- **!14724 — merged.** Corrects Zigbee Green Power scene-command naming; protocol correctness fix without a broader convention.
- **!14723 — merged.** Release-note update for Find Packet improvements; documentation-only.
- **!14722 — merged master, historical/superseded.** Adds dfilter `vals()` conversion. A post-merge crash report for `vals(udp)` exposed the overloaded `strings` member of `FT_PROTOCOL`; John Thacker points to later !14785 for the correction. Because !14785 was already reviewed and records the stronger final design, this MR is retained as historical/negative evidence only.
- **!14721 — merged master, review-significant.** In review of Ubiquiti Discovery additions, John Thacker requests a sample and points out that UDP 10001 is not registered to Ubiquiti and is assigned elsewhere by IANA. His review direction is that a shared/unregistered default port needs reliable protocol identification or should be left unconfigured by default. Useful heuristic/registration evidence, but not promoted in this run because existing dissector-detection guidance already covers false-positive avoidance.
- **!14720 — merged master.** Extends WSLua `DissectorTable:try()` to support `FT_NONE` payload/Decode-As tables via `dissector_try_payload()`. Corroborates API completeness: operations should support the table kinds that construction/registration declares valid.
- **!14719 — merged master.** Gerald Combs adds builtin syscall capture engines to falcodump with capability probing and failure warnings; substantial feature work but no distinct new convention promoted.
- **!14718 — merged master.** ASTERIX clang-tidy recursion suppressions document the termination argument (UDP-bounded input and monotonically advancing offset). Corroborates existing bounded-recursion/suppression guidance.
- **!14717 — merged master.** USBLL explicitly adds zero-length `usbll.data` for ZLPs so empty data is filterable. This strongly corroborates the existing empty-is-not-absent convention; later discussion also illustrates that changing field presence affects display-filter existence semantics.
- **!14716 — merged.** Logray profile-specific display-filter buttons; data/profile customization with no new cross-cutting rule.
- **!14715 — merged master.** Adds an X.75 payload `Decode As` table rather than forcing raw hexdump of a generic payload. John Thacker also reviews how the parent should treat the Info column when a subdissector owns interpretation. Corroborates existing extensibility/Decode-As guidance.
- **!14714 — merged master, extremely authoritative.** Guy Harris authors and merges further CLLog file-content correctness checking, extending structural and value validation of the Wiretap text reader. Promoted with !14713 to `wiretap-text-parsing-conventions.md`.
- **!14713 — merged master, extremely authoritative.** Guy Harris authors and merges CLLog numeric validation that replaces unchecked `sscanf()`-style parsing with checked Wireshark conversion helpers, uses storage matching the real domain, and turns malformed/out-of-range input into `WTAP_ERR_BAD_FILE` with useful error information. Promoted to `wiretap-text-parsing-conventions.md`.
- **!14712 — merged release-4.0, corroborating.** T.38 reassembly data outlives a packet, so it cannot live in `pinfo->pool`; the accepted fix uses explicit allocation tied to the resulting TVB with `tvb_set_free_cb()`. Corroborates existing allocator/lifetime-scope rules.
- **!14711 — merged release-4.2, corroborating.** Same T.38 lifetime fix on the other maintained branch.
- **!14710 — merged master, deep.** Primary T.38 lifetime fix by John Thacker. Forced reassembly bytes outlive the frame, so packet-scope allocation is replaced with storage freed when the owning TVB is freed. This is strong new evidence for the existing `allocator-scope-conventions.md`: choose the allocator/owner whose lifetime matches the actual consumer; packet relation alone does not make packet scope correct.

## Notebook changes promoted from this run

1. `protocol-context-state-conventions.md` — added the F1AP rule for reverse-order same-packet dependencies: retain only minimum deferred context in packet-owned protocol data, and never use static/global `proto_tree *` as a lifetime extension.
2. `ui-lifecycle-callback-conventions.md` — new rule that one framework layer must own each lifecycle transition; callers should invoke the operation rather than duplicating callbacks already produced by its event path.
3. `filesystem-file-identity-conventions.md` — new rule to compare underlying filesystem identity rather than path spelling when aliasing/hard links can affect correctness, and to perform collision checks before destructive opens.
4. `wiretap-text-parsing-conventions.md` — new Guy-Harris-weighted rule to use checked, domain-correct conversions for textual capture formats and reject invalid input through `WTAP_ERR_BAD_FILE` with useful context.

The T.38 series (!14710-!14712), Catapult !26567, Opus/USB zero-length work (!14729/!14717), dfilter !14722, recursion series, ETWDump !26568/!26569, and Decode-As work were retained as corroboration of already-existing notebook guidance rather than duplicating rules.

## Continuity

`mr_14709.json` exists at the same corpus commit, so the corpus is **not exhausted**. If no newer previously-unreviewed MRs are added before the next run, the next descending candidate is !14709. The scraping tool therefore does not need to be restarted.