# Wireshark MR review automation: !22793-!22842

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
- Review direction: newest to older
- Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, the aggregate automation ledger, and the individual files in `reviewed-mrs-automation/`; then selected the 50 highest-numbered corpus MRs not in that set. The previously reviewed !17571-!17620 batch remains counted.
- Exact reviewed set: !22842 through !22793 inclusive (50 MRs; every corpus JSON file in this range exists).

## Per-MR review notes

| MR | Review | Notes |
|---|---|---|
| !22842 | Scanned | Merged John Thacker SABP/asn2wrs dead-code cleanup. Generated `VAL_PTR` state was unused because Serial-Number handling already supplied the intended field; no additional durable convention. |
| !22841 | Deep | Merged John Thacker core-epan fix restoring `DISSECTOR_ASSERT` for negative `octet_aligned` inputs while signed callers were still being converted. Strong evidence that temporary signed API states need an always-visible dissector invariant until the type system excludes them; retained as corroboration rather than adding a potentially over-broad assertion rule. |
| !22840 | Scanned | Merged RDP failure-path cleanup frees an allocated buffer before returning NULL. Corroborates existing ownership/failure-cleanup guidance. |
| !22839 | Scanned | Public-header IWYU fixes following expanded header validation. Corroborates existing public-header self-containment/dependency rules. |
| !22838 | Scanned | GSM SIM FCP template support. Protocol feature addition with no reusable review lesson identified. |
| !22837 | Scanned | Geneve per-vendor option dissector dispatch. Accepted extension point; existing dissector-table guidance already covers the pattern. |
| !22836 | Scanned | Restores a const qualifier in Falco event code. Straightforward type-correctness cleanup. |
| !22835 | Deep | Broad FT_STRINGZ cleanup: avoids using string-extraction helpers merely to recover lengths and corrects string field usage. Strong corroboration for fetch-once/helper semantics and the !22834 wire-format field-type rule. |
| !22834 | Deep | Merged John Thacker correction: HTTP form URL-encoded keys/values are length-bounded packet strings, not NUL-terminated `FT_STRINGZ`. Added durable string field-type semantics to `protocol-field-semantic-conventions.md`. |
| !22833 | Scanned | Fixes warnings reported by repository checking scripts. Useful evidence for running project checkers, already covered by notebook tooling guidance. |
| !22832 | Scanned | RTCP/MCPC field naming correction. Small protocol-field cleanup; no new general rule. |
| !22831 | Deep | Core TVBuff API migration to unsigned offsets/lengths for `tvb_get_string_enc`; explicitly notes negative lengths were already invalid. Strong corroboration for unsigned-domain API guidance. |
| !22830 | Scanned | Coverity false-positive suppression in GSM A common. Reinforces the existing rule to distinguish analyzer limitations from real defects. |
| !22829 | Scanned | `mark_truncated` refactoring to make NUL-termination visible to Coverity. Analyzer-facing cleanup with no new general convention. |
| !22828 | Scanned | Multicast Router Discovery update to RFC 4286/IANA values. Standards refresh; no durable engineering rule beyond using current registries. |
| !22827 | Scanned | CI correction passes the disabled-plugin list through to CMake. Configuration propagation fix; no new general lesson. |
| !22826 | Scanned | Forticlient_connect payload dissection. Protocol-specific parsing addition. |
| !22825 | Deep | Corrects mismatched `proto_tree_add_*` calls found while working on IANA header checks. Strong corroboration that hf type/API combinations must match; existing field-registration/static-check guidance already captures it. |
| !22824 | Scanned | ISAKMP INTERNAL_DNS_DOMAIN attribute dissection. Protocol feature addition. |
| !22823 | Scanned | ISAKMP Fortinet Auto Negotiate/Keep Alive attributes. Protocol feature addition. |
| !22822 | Scanned | Initializes ciscodump `bytes_written` even though the current caller does not consume it. Straightforward initialization hardening. |
| !22821 | Scanned | BTMesh source reshaping to make an already-established initialization invariant understandable to Coverity. Corroborates analyzer-model guidance. |
| !22820 | Deep | Core charset-string APIs use `size_t` for lengths and remove narrowing casts around `strlen`. Corroborates size-domain/type guidance; no duplicate notebook rule added. |
| !22819 | Scanned | Handles a Megaco find-failure path reported by Coverity. Routine failure checking. |
| !22818 | Deep | Gadu-Gadu arithmetic cleanup avoids subtracting 1 from zero and subsequently adding it back. Strong corroboration for unsigned-underflow and intermediate-expression safety rules. |
| !22817 | Deep | Bitcoin uses `tvb_reported_length_remaining()` rather than open-coded reported-length minus offset arithmetic. Strong corroboration for TVBuff remaining-length helpers and underflow avoidance. |
| !22816 | Scanned | Initializes proto-registrar statistics state. Straightforward initialization fix. |
| !22815 | Scanned | KNX/IP decrypt checks a failure result before consuming output that would otherwise be uninitialized. Corroborates error-return checking. |
| !22814 | Scanned | TPM 2.0 NV_Read handle-count correction. Protocol-specific correctness fix. |
| !22813 | Scanned | IXIA trailer LEARNED-field support. Protocol feature addition. |
| !22812 | Scanned | Const-qualifies additional static data, moving it from writable to read-only storage. Useful cleanup but no new notebook convention. |
| !22811 | Scanned | ISAKMP configuration attributes refreshed from the IANA registry. Standards-data maintenance. |
| !22810 | Scanned | Fortinet DNS Suffix ISAKMP attribute. Protocol feature addition. |
| !22809 | Scanned | Removes unnecessary const casts. Type-hygiene cleanup. |
| !22808 | Deep | Merged John Thacker openSAFETY overflow fix: small unsigned integer operands are still subject to C integer promotions and may be evaluated as signed `int`. Added durable promoted-expression guidance to `arithmetic-safety-conventions.md`. |
| !22807 | Scanned | SPOOLSS overflow hardening from Coverity findings. Corroborates existing checked/range-safe arithmetic guidance. |
| !22806 | Scanned | Corrects diagnostic pragmas intended to re-enable cast-qual warnings. Build-warning hygiene; no distinct convention. |
| !22805 | Deep | TVBuff text-formatting APIs converted to unsigned offsets/lengths after confirming repository callers do not rely on negative sentinels. Corroborates API-domain migration guidance. |
| !22804 | Deep | `tvb_strn[case]eql`/`tvb_memeql` offsets and sizes converted to unsigned domains; negative sentinel behavior was not used. Corroborates unsigned-domain API guidance. |
| !22803 | Scanned | Adds platform-aware plugin-filename helper and macOS plugin disabling. Platform/plugin maintenance; existing plugin conventions suffice. |
| !22802 | Discussion-focused (open snapshot) | Protocol Hierarchy fix filters same-protocol candidates by actual tree path. Pipeline was green but corpus snapshot still shows the MR open with no substantive maintainer review, so it is deliberately down-weighted and not promoted as settled architecture. |
| !22801 | Scanned | Bluetooth L2CAP BR/EDR Security Manager fixed-channel update. Protocol/UI behavior update; no new general convention. |
| !22800 | Deep | Merged CI test fix spun out of a larger MR: enables the stdin-capture test correctly and makes assertions inspect the proper diagnostic stream, including a username containing `-k`. Corroborates focused-MR and environment-sensitive test guidance. |
| !22799 | Scanned | Removes an RTSP dead store found by Clang Analyzer. Static-analysis cleanup; no distinct rule. |
| !22798 | Scanned | Removes obsolete TVBuff comment after remaining-length APIs had long returned 0 for out-of-bounds offsets rather than -1. Documentation aligned with actual API semantics. |
| !22797 | Deep | Adds internal unsigned variants of TVBuff contiguous-range helpers as part of the broader signed-to-unsigned migration. Corroborates API-domain and bounds guidance. |
| !22796 | Scanned | NMEA2000 spelling/documentation fixes. No engineering convention. |
| !22795 | Scanned | Initializes several Qt members reported by Coverity. Straightforward initialization hardening. |
| !22794 | Scanned | Clarifies an intentionally unused Echo dissector parameter. Cosmetic/static-check clarity change. |
| !22793 | Scanned | KNX/IP decrypt printf-format correction uses the format corresponding to integer promotion. Type/format hygiene; related to, but weaker than, the promoted-expression lesson in !22808. |

## Durable notebook changes from this run

1. `arithmetic-safety-conventions.md`: added the rule that small unsigned integer types can be promoted to signed `int`, so arithmetic safety must be reasoned about using the expression's promoted type rather than the variable's declared typedef. Primary evidence: merged !22808, authored and merged by John Thacker.
2. `protocol-field-semantic-conventions.md`: added the rule that `FT_STRINGZ` is a wire-format semantic (a NUL terminator actually present in the packet), not merely a host-string representation. Primary evidence: merged !22834, authored and merged by John Thacker; !22835 independently corroborates the surrounding cleanup.

No Guy Harris review comment in this selected batch supplied a stronger contradictory architectural rule that warranted promotion. Open !22802 was deliberately down-weighted relative to merged maintainer-authored changes.
