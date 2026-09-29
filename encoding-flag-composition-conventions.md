# Encoding Flag Composition Conventions

Merged master MR !5509, authored by João Valverde, fixes pre-commit errors across many dissectors by removing the neutral encoding marker from calls that already specify a concrete character encoding.

**Rule:** a concrete character encoding is the field's encoding contract. The neutral marker is for cases where encoding is not applicable; it is not an additive modifier to a real text encoding. Use the one type-appropriate encoding expected by the tree API.

**Tooling rule:** treat encoding-argument pre-commit checks as API-contract validation rather than cosmetic style enforcement.

**Confidence:** High; broad merged master cleanup driven by Wireshark's pre-commit validation.
