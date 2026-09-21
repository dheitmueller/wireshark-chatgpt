# Wireshark ABI Compatibility Conventions

This file records durable ABI-compatibility conventions extracted from accepted upstream Wireshark changes. Current upstream source, symbol-versioning policy, and maintained-branch requirements remain authoritative.

## Preserve exported symbols for the lifetime of a stable-branch ABI

Removing an implementation from active use does not automatically make its exported symbol safe to remove from a maintained release branch. If an already-released public symbol is part of that branch's ABI, keep the symbol available until the compatibility boundary permits its removal, even when the implementation can only be retained as a compatibility stub.

Merged release-4.4 MR !23538, authored by Gerald Combs and approved by John Thacker, restores `ws_base32_decode()` after its removal broke ABI compatibility. The accepted fix restores both the public declaration and a stub implementation specifically so binaries built against the stable branch continue to resolve the symbol.

**Implementation rule:** before deleting or renaming a public/exported function on a maintained branch, check the branch's ABI contract rather than reasoning only from source-tree call sites. If compatibility requires the symbol, retain an ABI-compatible declaration and implementation/stub until the next allowed ABI break; remove it only at an intentional compatibility boundary.

**Confidence:** Very high. Merged supported-branch ABI repair authored by Gerald Combs and approved by John Thacker, with the compatibility failure stated explicitly in the MR.

## Adding read-only `const` qualification is not inherently an ABI break

Do not treat every source-level type qualifier change on an exported C function as though it changes the binary calling convention. When a pointer argument is semantically read-only, adding `const` to the pointed-to type can make the public declaration accurately express the implementation's contract without changing how the pointer is passed at the ABI level.

Merged master MR !14257 const-ifies generated registration tables and changes exported `register_all_tap_listeners(tap_reg_t *)` to `register_all_tap_listeners(tap_reg_t const *)`. The author explicitly raised concern about modifying a `WS_DLL_PUBLIC` signature. Guy Harris replied that he knew of nothing that would cause API or ABI breakage from adding a `const` qualifier to an argument or to the target of a pointer argument, noted that callers cannot legitimately depend on a routine modifying data it now promises not to modify, and then approved the MR. He also cautioned that Wireshark does not promise strong API/ABI compatibility between major releases.

**Review rule:** distinguish binary ABI, source/API typing, and semantic mutation contracts. For a public pointer parameter that the callee does not modify, adding pointee `const` is generally compatible with the C calling convention and improves the source contract; nevertheless, review unusual uses such as function-pointer type matching and the compatibility policy of the branch being changed rather than applying a blanket rule to every qualifier edit.

**Confidence:** Extremely high for the stated Wireshark review precedent. The compatibility question was answered directly by Guy Harris on a merged MR and followed by his explicit approval.