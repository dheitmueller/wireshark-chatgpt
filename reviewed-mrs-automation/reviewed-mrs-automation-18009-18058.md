# Automated Wireshark MR review: !18009-!18058

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

## Exact reviewed set

!18009, !18010, !18011, !18012, !18013, !18014, !18015, !18016, !18017, !18018, !18019, !18020, !18021, !18022, !18023, !18024, !18025, !18026, !18027, !18028, !18029, !18030, !18031, !18032, !18033, !18034, !18035, !18036, !18037, !18038, !18039, !18040, !18041, !18042, !18043, !18044, !18045, !18046, !18047, !18048, !18049, !18050, !18051, !18052, !18053, !18054, !18055, !18056, !18057, !18058.

Count: 50.

Selection was made after consulting the existing per-run automation ledgers and `reviewed-mrs.md`. The historical !17571-!17620 batch remains part of the already-reviewed set. Review direction remains descending MR number; the next candidate frontier is below !18009, subject to rebuilding the exact already-reviewed set rather than assuming contiguous coverage.

## Weighting and durable findings

Merged MRs were weighted above abandoned/superseded work. This batch did not justify a new convention file; the strongest findings corroborate existing notebook guidance.

- !18020 (merged, Martin Mathieson review): eCPRI field-registration design discussion favored sharing one filterable `hf_` field when the protocol-level semantic field is genuinely common across message types, rather than embedding separate hf registrations in per-message structs. This reinforces semantic display-filter identity and avoiding unnecessary registration fragmentation.
- !18010 (merged, John Thacker): capture-filter configuration is read only when the selected option actually requires predefined filters, and is not reread in the capture child after the parent has already resolved the name. This is a strong initialization/layering example: avoid duplicate configuration work and keep parent/child responsibilities explicit.
- !18009 (merged): `packet_info.rel_ts` is kept relative to the first packet so taps and graphs retain stable calculation semantics even when packets are marked as time references. This reinforces keeping core data semantics independent of presentation/reference choices.
- !18040 (merged release backport, John Thacker): a compiler warning from Lemon-generated code is suppressed narrowly for the affected generated-code/compiler case because the always-true comparison is intentional. This reinforces fixing or suppressing warnings at the correct generator/tool boundary rather than distorting generated semantics merely to silence diagnostics.
- !18050 and !18058 (merged master + release-4.2 backport): HSRP TLV tree-item display spans include the complete TLV (header plus value), while the displayed protocol length remains the TLV value length. This corroborates accurate tree byte-range ownership and keeping wire-format span distinct from a protocol length field's semantic value.
- !18030 (merged): SOME/IP changed Info-column handling from replacing the column for each message to appending message details, allowing multiple SOME/IP messages in one packet to remain visible; it also exposes the dissector for invocation by other dissectors/Lua. This reinforces composable dissectors and additive column behavior for multi-message containers.

## Notebook update decision

No convention file changed in this run. The findings above substantially overlap rules already represented in the notebook (semantic hf/filter identity, initialization/layering, stable analysis semantics, generated-code/tool warning handling, accurate byte-range ownership, and composable dissector behavior), so adding duplicate prose would reduce signal.
