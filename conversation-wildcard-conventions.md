# Wildcard Conversation Identity

## UDP wildcard conversations are not automatically improved by filling every endpoint

Merged master MR !7163, authored by John Thacker, fixes TFTP servers that keep the well-known port. The accepted logic searches the wildcarded UDP conversation in the correct direction and verifies that TFTP owns it. It deliberately avoids always converting the wildcard to an exact server port because generic conversation lookup prefers exact tuples, while port reuse can require choosing the most recent semantic conversation instead.

**Rule:** before converting a wildcarded connectionless-protocol conversation to an exact tuple, account for direction, protocol ownership, tuple reuse, frame/recency semantics, and lookup precedence. More specific identity is not necessarily more correct.

**Confidence:** Very high. Merged master conversation-state fix authored by John Thacker.
