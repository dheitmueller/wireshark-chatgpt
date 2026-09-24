# Wireshark Registered Identity and Static Registration Conventions

This file records durable conventions for protocol, field, subtree, and expert-registration identities extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Compare registered identities symbolically, not against incidental numeric assignments

Numeric IDs assigned by protocol/field registration are implementation details. Code that needs to recognize a particular registered object should compare against the named ID/handle for that object rather than assuming the registry will assign a particular integer such as zero.

Merged master MR !11156 fixes JSON protocol-tree output that identified the pseudo text-only field with `fi->hfinfo->id == 0`. The value of `hf_text_only` happened to be zero after `proto_init()`, but that was only an incidental assignment and could change. The accepted code compares directly with `hf_text_only`. Release-4.0 backport !11157 carries the same correction and was approved/merged by Gerald Combs.

**Implementation rule:** if an object has a registered symbolic identity (`hf_*`, `proto_*`, `ett_*`, `ei_*`, a dissector handle, or another registry token), use that identity for equality and dispatch. Do not encode assumptions about the allocator's current numbering or pre-registration sentinel value into unrelated consumers.

**Review implication:** bulk registration/lifecycle refactors are a reason to search for magic numeric comparisons against registered IDs. A check can keep working by coincidence until registration order or initialization policy changes.

**Confidence:** Very high. Merged master correctness fix plus stable-branch backport; the failure mode and incidental-zero assumption are stated directly in the MR.

## Use implicit zero initialization for static registration variables only when zero is the defined pre-registration state

Static-storage objects are already zero-initialized by C. Once the registration APIs deliberately support zero as the pre-registration/default state for protocol, header-field, expert-info, and subtree variables, spelling out thousands of explicit zero initializers only forces unnecessary initialized data into the binary. Conversely, variables whose required default is a nonzero sentinel must retain an explicit declaration-time initializer.

Merged master MR !11155 introduced tooling to convert the relevant static registration variables from `-1` to zero and reported roughly 1 MiB of static initialization savings. Merged master MR !11158 then removed the redundant explicit initializers entirely; its description reports approximately 1,266,320 bytes of binary-size reduction by allowing those zero-valued statics to reside in `.bss`. This complements, rather than contradicts, the later Guy Harris rule from !11725: declaration-time initialization remains important for global state whose valid default is a nonzero/sentinel value.

**Implementation rule:** distinguish semantic default from storage syntax. For static registration variables whose API-defined initial state is zero, rely on C's implicit zero initialization unless current upstream style requires otherwise. For nonzero/sentinel defaults, initialize explicitly at declaration so every entry point starts in a valid state.

**Review implication:** do not mechanically remove initializers from arbitrary globals. First verify that zero is the intended semantic default and that no code relies on an older `-1`-style sentinel. After such a migration, audit consumers for magic comparisons that conflate the new storage default with a specific registered object.

**Confidence:** High. Two merged master changes intentionally apply the same storage model at broad scale, with measured binary-size rationale; !11156 independently demonstrates why consumers must not mistake the zero initialization value for a stable registered identity.
