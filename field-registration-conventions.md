# Wireshark Field-Registration Conventions

This file records durable protocol-field registration and value-display conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Use the dynamic value-string indirection form only for value_string_ext objects that actually change at runtime

`VALS_EXT_PTR()` exists for `value_string_ext` pointers whose target is dynamically selected or replaced at runtime. Static `value_string_ext` objects do not need that extra indirection.

Merged master MR !23656, authored and merged by Michael Mann, removes widespread copied uses of `VALS_EXT_PTR()` where the `value_string_ext` is actually static and passes the address of the static extension directly instead. The MR explicitly notes that copy/paste had allowed the dynamic form to proliferate beyond its intended use.

**Implementation rule:** for a static `value_string_ext`, register the field with the address of that extension directly. Reserve `VALS_EXT_PTR()` for cases where the pointer itself is genuinely dynamic at runtime.

**Review rule:** do not preserve a more indirect field-registration form merely because nearby dissectors use it. Verify whether the underlying table or its pointer is truly dynamic; copied registration boilerplate is not evidence of the intended API contract.

**Confidence:** Very high. Merged master cleanup authored and merged by senior maintainer Michael Mann, explicitly documenting the intended distinction.
