# Wireshark Field Deregistration Lifecycle Conventions

This file records durable lifecycle guidance for dynamically deregistered protocol fields. Current upstream source remains authoritative.

## Do not free deregistered fields from dissectors or preference callbacks

Calling `proto_deregister_field()` does not make the corresponding field metadata immediately safe to free. Other Wireshark state may still hold references to the field, including while a dissector preference callback is running. `proto_free_deregistered_fields()` is therefore an owning-framework lifecycle operation and must not be called by dissectors, including from preference callbacks; the framework will invoke it when references are no longer live.

When dynamically allocated companion data must have the same lifetime as deregistered fields—for example, an array containing `hf_id` pointers—register that allocation with `proto_add_deregistered_data()` instead of trying to force immediate field destruction.

**Evidence:** merged master MR !19260, authored and merged by John Thacker, documents this contract directly in `proto.h` after issue #20425 exposed the unsafe lifetime assumption.

**Implementation rule:** distinguish deregistration from destruction. A dissector may remove a field from future registration use at the supported lifecycle point, but ownership of the underlying registered-field objects remains with epan until the framework reaches its safe reclamation phase. Tie related dynamically allocated metadata to that deferred reclamation mechanism rather than freeing either side early.

**Review implication:** preference callbacks are not a safe teardown boundary merely because they are where a dynamic field set changes. When reviewing code that rebuilds dynamic `hf` arrays, look for immediate calls to global/freeing registration APIs and verify that stale references cannot survive the callback.

**Confidence:** Extremely high. The rule is stated directly in a merged API documentation change authored and merged by John Thacker, with the unsafe caller class and supported replacement mechanism named explicitly.
