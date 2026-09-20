# Wireshark Const-Correctness Conventions

This file records durable conventions for using C `const` to express read-only ownership and make Wireshark interfaces compose safely. Current upstream APIs remain authoritative.

## Mark shared registered metadata read-only through observer objects

A pointer reachable through a per-packet or per-instance wrapper does not imply that the pointed-to object belongs to that wrapper. When it refers to globally registered metadata shared by many instances, expose it as `const` through observer structures so callers cannot accidentally mutate global registration state through a local view.

Merged MR !15469, authored by John Thacker and merged by Anders Broman, changes `field_info` so its `header_field_info` pointer is const-qualified. The MR explains that the HFI is registered information shared by all fields using that HFI and should never be modified through an individual `field_info`. There was no known writer to remove; the purpose of the change was to make the ownership invariant enforceable by the type system.

**Implementation rule:** const-qualify pointers to shared registration/configuration metadata when the current object is only an observer. Prefer making an ownership invariant explicit in the type over relying on the fact that current callers happen not to write through the pointer.

## Lookup APIs should not require mutable context when they only inspect it

A lookup operation that conceptually reads packet context should accept a pointer to const context when practical. Requiring a mutable pointer unnecessarily prevents use from const-qualified callbacks and suggests side effects that the operation does not perform.

Merged MR !15468, authored and merged by John Thacker, const-qualifies the `packet_info` parameter of conversation lookup functions that only find existing conversations. The immediate benefit was composability with reassembly fragment-key callbacks whose contract already provides `const packet_info *`; those callbacks can now perform conversation lookup without discarding constness.

**Implementation rule:** make read-only lookup/query APIs accept const-qualified inputs when their semantics permit it. Do not cast away const merely to call an API that does not actually mutate its argument; correct the API boundary instead when the ownership contract is under Wireshark's control.

**Review rule:** treat const-correctness as architecture rather than cosmetic style. Check whether pointer mutability reflects who owns the object and whether a query operation truly needs mutation; improving those contracts can prevent accidental writes and remove friction between otherwise compatible callbacks.

**Confidence:** Very high. Two merged master API cleanups authored by John Thacker, one merged by Anders Broman and one authored/merged by John, each with an explicit ownership/composability rationale.
