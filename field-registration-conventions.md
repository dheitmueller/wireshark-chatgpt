# Wireshark Field-Registration Conventions

This file records durable protocol-field registration and value-display conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Use the dynamic value-string indirection form only for value_string_ext objects that actually change at runtime

`VALS_EXT_PTR()` exists for `value_string_ext` pointers whose target is dynamically selected or replaced at runtime. Static `value_string_ext` objects do not need that extra indirection.

Merged master MR !23656, authored and merged by Michael Mann, removes widespread copied uses of `VALS_EXT_PTR()` where the `value_string_ext` is actually static and passes the address of the static extension directly instead. The MR explicitly notes that copy/paste had allowed the dynamic form to proliferate beyond its intended use.

**Implementation rule:** for a static `value_string_ext`, register the field with the address of that extension directly. Reserve `VALS_EXT_PTR()` for cases where the pointer itself is genuinely dynamic at runtime.

**Review rule:** do not preserve a more indirect field-registration form merely because nearby dissectors use it. Verify whether the underlying table or its pointer is truly dynamic; copied registration boilerplate is not evidence of the intended API contract.

**Confidence:** Very high. Merged master cleanup authored and merged by senior maintainer Michael Mann, explicitly documenting the intended distinction.

## Pass semantic display-base values, not unrelated integers that happen to fit the parameter

Field-display parameters such as the `encoding`/display-base argument consumed by protocol-tree helpers are semantic enums, not arbitrary numeric slots. A literal or derived integer can compile and even accidentally equal a valid enum constant while still expressing the wrong contract. Review the registered field's intended display semantics and pass the appropriate `BASE_*` value rather than a bit width, byte width, or other coincidentally compatible integer.

Merged master MR !16507, authored and merged by John Thacker, fixes 6LoWPAN fields passed to `proto_tree_add_bits_item()` where the final argument was `16`, evidently intended to describe field width. That parameter actually controls display representation; numeric 16 coincided with an unrelated display-base enum value, so type checking did not reveal the mistake. The correction uses the intended `BASE_DEC`, with release-branch backports !16518 and !16519 carrying the same fix.

**Implementation rule:** treat field-registration and protocol-tree display parameters according to their declared semantic enum domain. Never substitute a size or width merely because the underlying C type accepts it; prefer named enum constants so the intended behavior is reviewable and resilient to API changes.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by John Thacker and propagated to two stable branches.

## Match 64-bit value strings with the required display flag

In the merged WoW dissector MR !9744, CI explicitly reported a field using `VALS64` whose display did not include `BASE_VAL64_STRING`; the author corrected it. The 32-bit `VALS` registration behavior does not automatically carry over to 64-bit lookups.

**Rule:** Register value-string lookup width, `FT_*` type and display-base flags consistently. Treat dissector checker errors as semantic API-contract failures. **Confidence:** Moderate/high from explicit CI diagnostic and author discussion; the corpus lacks the final oversized diff.

## Never reuse one filter abbreviation for incompatible field types

Merged master !9694 corrected the MySQL `COM_BINLOG_DUMP_GTID` position, which is an eight-byte value rather than the four-byte field used by a different command. Alexis La Goutte recommended a separate header field and then caught the proposed use of the same abbreviation `mysql.binlog.position` for both `FT_UINT32` and `FT_UINT64`: `proto_register_field_init()` rejects incompatible duplicate abbreviations. The contributor corrected the submission before merge and supplied replication sample traffic.

**Field rule:** field identity includes its type and wire semantics. When two command variants genuinely encode different widths, give them appropriately registered, distinguishable filter names; don't assume two different `hf_` identifiers make a duplicate filter abbreviation legal. Test both variants and field-registration initialization. **Confidence:** very high; explicit Alexis review and merged correction.

Merged master !9703 further corrects RDMnet and MS NLB fields whose registered `FT_UINT8` types were narrower than their actual 32- and 16-bit encodings. **Checker rule:** use `check_typed_item_calls.py` to find mismatches, fix the actual width, and make narrowly justified checker exceptions rather than suppressing valid diagnostics. **Confidence:** high.

## Keep typed-item checker exceptions narrow and fix underlying wire-width errors

Merged master !9631, !9636 and !9655 resolve large sets of field-size/mask inconsistencies; !9629 adds narrowly scoped regex exceptions for known legitimate repeated filter identifiers while fixing real collisions (including distinct TEAP and NETLOGON abbreviations). Martin Mathieson's !9655 also received John Thacker's reminder that an ASN.1-generated mismatch must be fixed at the template/source level (see later corrective !9892), not only in the generated C file.

**Checker rule:** run `tools/check_typed_item_calls.py` and field-registration checks after adding fields. Correct the registered `FT_*` type, item width, field mask, and unique filter identity to match the wire representation; suppress only a demonstrated false positive with a specific auditable exception. For generated files, update the generator/template and regenerate. **Confidence:** high from multiple merged changes, with direct John reviewer guidance.

Merged VITA 49 CIF1 extension !9620 drew Alexis La Goutte's warning that a Boolean field item was added with an eight-byte length incompatible with the field's `FT_BOOLEAN` width. In merged GREBonding fix !9637, Martin Mathieson highlighted a field-registration warning for two conflicting `value_string` entries with the same code. **Review rule:** check *item span versus registered field width* and uniqueness/semantics of enumerated values, not just whether the code compiles; inspect reviewer-identified warnings before treating a merged patch as verified in every detail. **Confidence:** high for the explicit checker findings and merged fixes.

## Encode nonzero flags as Booleans and keep exported metadata fields scoped

In merged !9597, Pascal Quantin advised that fields specific to the TCP dissector-data tag in Exported PDU should use names such as `exported_pdu.tcp_dissector_data.*`, not ambiguous global `dissector_data.*` identifiers. A flag documented as “nonzero means reassembled” belongs in `FT_BOOLEAN` and a typed `proto_tree_add_item_ret_boolean()` call, rather than implying that only 0/1 integer values are valid. Pascal also warned against passing a narrow return type's address to an API expecting `guint32 *`, which is unsafe across architectures. Unsupported tag versions should be expert diagnostics, not assertions.

Merged dissector submissions !9581 and !9583 carry additional Alexis La Goutte and Martin Mathieson review evidence: duplicated abbreviations with incompatible `FT_*` types, a 4-byte `proto_tree_add_item` span with `FT_UINT8/FT_UINT16` registration, and uncertain table bindings must be corrected before considering a new dissector complete. **Rule:** separate displayed semantics from on-wire backing, choose an API whose result type matches the destination object, and run type/abbreviation checkers alongside sample dissection. **Confidence:** high; accepted implementation plus direct senior reviewer feedback.

## `FT_FRAMENUM` uses `strings` as a semantic relationship tag, not a value table

John Thacker's merged master !9490 documents `FRAMENUM_TYPE(x)` in `doc/README.dissector`: `FT_FRAMENUM_REQUEST`, `RESPONSE`, `ACK`, `DUP_ACK`, `RETRANS_PREV`, `RETRANS_NEXT`, and `NONE` identify the relationship represented by the frame-number field. The packet list uses these tags to select related-packet symbols; a NULL `strings` pointer means `FT_FRAMENUM_NONE`. Merged master !9487 fixes a real custom-column crash because `proto_custom_set()` had interpreted `FT_FRAMENUM`'s non-NULL `strings` as an ordinary numeric value-string mapping and attempted `hf_try_val_to_str*`.

**Rule:** code examining `header_field_info.strings` must branch by the registered `FT_*` type before assuming that the pointer is a `value_string`/lookup table. Preserve the semantic relationship metadata for packet-list navigation, while outputting the underlying frame number in custom columns. Add regression coverage for each non-NONE frame relationship as a `-T fields`/custom-column value. **Confidence:** very high, two merged John Thacker master changes, one explanatory API document and one observed crash fix.

## Boolean presentation requires a 0/1 wire domain; typed-item widths must match the packet

In merged OAM/DPoE master **!8967**, **Alexis La Goutte** suggested existing `tfs_...` true/false strings. The author correctly explained why a field with values `disabled=1` and `enabled=2` cannot use a representation expecting values `0` and `1`. Alexis's typed-item checker also found a two-byte tree insertion against an `FT_UINT8` declaration; the author changed the field to `FT_UINT16` to match the specification. In merged GSM RACH **!8995**, Alexis recommended `value_string` or `range_string` rather than a manual enumeration switch.

**Rule:** select a Boolean/value/range mapping based on the actual *wire values*, not the English labels; do not turn an arbitrary two-valued enumeration into an `FT_BOOLEAN`. Cross-check type, width, bitmask and encoding in every typed `proto_tree_add_item` call. Prefer conventional mapping tables when an enumeration can be expressed declaratively. Run checker tooling on new fields and verify with a specimen packet.

**Confidence:** Very high for Alexis's explicit review comments and merged correction; not an assertion that all two-state flags use one representation.
