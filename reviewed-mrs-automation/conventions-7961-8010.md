# Durable conventions from Wireshark MRs !7961-!8010

Corpus snapshot: ddcaa22b51c68f594e425a23388c3a2086813054

## TVBuff and protocol-tree behavior

MR !8005 establishes that reported length must not be smaller than captured length. MR !8003 shows that TVBuff search helpers should normalize offset and limit conventions at entry and then use one bounded coordinate system internally.

MR !8008, with release backport !8009, establishes that skipping unreferenced protocol-tree fields is only a rendering optimization. Expert information and other semantic outputs must still be produced.

## Persistent state and conversation identity

MR !7982 fixes an HTTP persistent-map key that depended on temporary packet-local storage. Persistent containers must use keys whose identity and lifetime match the container; when an integer is the complete identity, direct integer keying is preferable to retaining a temporary address.

Guy Harris's !7970 and !7966 distinguish arbitrary conversation-element identity from the separate address/port endpoint mechanism. API and structure names should describe the actual identity model rather than using a generic “conversation key” label.

## Public API and ABI compatibility

MR !7996 restores a public conversation helper because external users may depend on it and restores its exported-symbol entry at the same time. MR !7977 re-enables ABI checking against release-specific baselines and preserves reports from all library comparisons before returning failure. MR !7976 shows that ABI tooling is sensitive to compiler/debug-info changes, so the CI environment should make those choices explicit.

## Parser progress

MR !7981 rejects invalid F5 trailer lengths/types and exits when a child consumes no bytes. MR !7961 validates a structural minimum PDU size before repeated parsing, while !7965 prevents a zero-length reassembly insertion. Every recoverable variable-length loop iteration must either make positive progress or terminate.

## Bounded byte decoding and semantic naming

John Thacker's !7967 extends percent decoding to an explicit pointer-plus-length interface so selected bytes need not be NUL-terminated and may include internal NULs.

Guy Harris's !8010 renames a shared AppleTalk/DSI context field from a misleading sequence-number name to the transaction/request-ID meaning actually shared by its producers and consumers. Shared handoff structures should use terminology valid for every protocol path that uses them.

## Submission hygiene

Closed !7999 is negative evidence: the new dissector had multiple checker failures and arrived on a branch with unrelated history that was difficult to rebase and squash. Run the standard dissector checks before submission and keep topic branches narrowly scoped.

MR !7972 also reinforces direct-header discipline: code using a standard-library function should include the header that declares it rather than relying on transitive includes.
