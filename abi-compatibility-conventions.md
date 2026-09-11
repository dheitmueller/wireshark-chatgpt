# Wireshark ABI Compatibility Conventions

This file records durable ABI-compatibility conventions extracted from accepted upstream Wireshark changes. Current upstream source, symbol-versioning policy, and maintained-branch requirements remain authoritative.

## Preserve exported symbols for the lifetime of a stable-branch ABI

Removing an implementation from active use does not automatically make its exported symbol safe to remove from a maintained release branch. If an already-released public symbol is part of that branch's ABI, keep the symbol available until the compatibility boundary permits its removal, even when the implementation can only be retained as a compatibility stub.

Merged release-4.4 MR !23538, authored by Gerald Combs and approved by John Thacker, restores `ws_base32_decode()` after its removal broke ABI compatibility. The accepted fix restores both the public declaration and a stub implementation specifically so binaries built against the stable branch continue to resolve the symbol.

**Implementation rule:** before deleting or renaming a public/exported function on a maintained branch, check the branch's ABI contract rather than reasoning only from source-tree call sites. If compatibility requires the symbol, retain an ABI-compatible declaration and implementation/stub until the next allowed ABI break; remove it only at an intentional compatibility boundary.

**Confidence:** Very high. Merged supported-branch ABI repair authored by Gerald Combs and approved by John Thacker, with the compatibility failure stated explicitly in the MR.