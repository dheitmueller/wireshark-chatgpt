# Convention synthesis: !3361–!3410

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## Timestamp and interface metadata follow the data source that actually owns them
Guy Harris's merged !3387 and !3399, with stable-branch backports !3388/!3389 and !3401/!3402, establish that timestamp precision must follow the component that actually supplies the timestamp. A LINKTYPE_ERF pcap wrapper does not make the timestamp a pcap timestamp; the ERF record supplies it, so the Wiretap record and any ERF-derived IDB must carry ERF precision. Guy-authored !3394/!3395 apply the same provenance idea to interface metadata: do not synthesize an outer-wrapper IDB when the encapsulated record stream contains the authoritative interface information.

## Respect option cardinality and choose add versus set deliberately
Merged Guy-authored !3409 documents the Wiretap block-option contract. For an option that the pcapng specification permits only once, add is the operation for creating the absent instance and set is the operation for changing an existing instance. For options that may repeat, add creates another occurrence and set_nth updates a particular existing occurrence. Review code that uses add/set as a semantic operation, not as interchangeable convenience functions.

## Normalize common container mechanics before subtype dispatch
Merged Guy-authored !3372 centralizes tolerated block-length alignment normalization; !3373 centralizes option-header reading, option-length bounds, content reading, and padding. Subtype callbacks should consume normalized semantic inputs rather than each reconstructing generic pcapng framing.

## Propagate nested failures and clean up before unwinding
Merged Guy-authored !3396 changes ERF helpers to return failure and carry err/err_info through the full call chain. Callers stop consuming partially valid state and free temporary lists/arrays on failure. Invalid internal preconditions are reported as WTAP_ERR_INTERNAL rather than being ignored until a later secondary failure.

## Diagnostics should describe the real failure site
Merged Guy-authored !3404 avoids attaching the exception-handler source location to a delayed dissector-bug report, because that location is identical for every such failure and is not where the fault occurred. Prefer diagnostics that preserve the relevant failure context over mechanically including the location of generic reporting machinery.

## Guard expensive debug construction with runtime log activation
Merged !3393, shaped by João Valverde review, keeps debug code available at runtime instead of compiling it out locally, and guards an allocating hex-dump path with ws_log_message_is_active(). This prevents dead/bit-rotted debug code while avoiding work when the domain/level is inactive.

## Preserve parent parser consumption semantics when inserting heuristic dispatch
Merged !3366 exposed a subtle integration bug: the previous direct child path advanced the parent ICMPv6 offset, while the new successful heuristic path initially did not. When replacing direct dispatch with a heuristic table, preserve the caller's byte-consumption/cursor contract on every success path. Also preserve parent protocol presentation for nested protocols by appending protocol-column text rather than replacing it.

## Keep mergeable history free of experiments
In merged !3367, an experimental commit named "test" accidentally entered the accepted history and then required merged !3376 to revert it. Stig Bjørlykke explicitly flagged the opaque commit. Before submission/merge, squash or remove experimental commits and ensure each retained commit describes an intended reviewable change.

## Weight superseded evidence appropriately
Closed !3405 contains useful design discussion, including Lars Völker's push to share CAN dispatch across all carriers, but it explicitly continues in later merged !3668; the merged successor is authoritative. Closed !3406 is useful history for conversation-based reassembly but is weaker than later merged John Thacker work. Merged !3370 contains valuable Guy Harris criticism, but the reviewed behavior itself needed later correction, so retain the critique rather than treating the flawed placement as precedent.
