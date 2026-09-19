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
