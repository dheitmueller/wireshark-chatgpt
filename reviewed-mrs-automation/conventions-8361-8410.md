# Durable conventions from !8361–!8410

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## Shared metadata for language bindings

Merged !8362, authored and merged by João Valverde, moves WSLua constant generation from a separate header-parsing script to Wireshark introspection metadata. Language bindings should consume the same enum and constant metadata as native code instead of re-parsing headers and duplicating semantic knowledge. Related Lua constants should stay grouped in structured namespaces.

## Wire lengths and decoded text lengths are different units

Merged !8368, authored and merged by João Valverde, fixes RPC code that used a wire-format byte count to truncate decoded UTF-8. Packet offsets and lengths remain in the wire representation; operations on converted text must use the decoded representation. Merged !8379 and !8402, authored by John Thacker, reinforce this by replacing manual or raw string extraction with encoding-aware TVBuff APIs.

## Installed data must be audited across packaging paths

Merged !8369 adds a RADIUS dictionary. Alexis La Goutte required the contributor to add the resource to the Windows installer manifests and to check whether the other Windows packaging path also needed an explicit update. Adding a peer runtime data file requires checking the runtime index plus every installer or package manifest that ships comparable files.

## Keep useful warnings visible when relaxing fatality

Merged !8398 and !8400, authored by João Valverde, add and use a local warning-demotion mechanism for a deprecated Qt API so the diagnostic remains visible without failing the build under the general warning-as-error policy. Merged !8394 applies the same distinction at build-system scope for optimizer-dependent warning classes prone to compiler false positives. Prefer local demotion for a specific call site and broader non-fatal treatment only when the warning class itself is unreliable across the tree.

## New dissector review should combine automated checks with captures

Merged !8371 received detailed review from Alexis La Goutte. Review and CI caught a duplicate field, incorrect masked-Boolean metadata, an out-of-order extended value-string table, naming issues, and the absence of a sample capture. The contributor supplied a pcap and fixed the issues before merge. Masked Boolean fields must describe the parent field width correctly, and extended value-string tables intended for optimized lookup must remain sorted.

## Weight superseding merged changes over abandoned drafts

Closed !8367 is an untested predecessor to merged !8371, and closed !8384 is superseded by merged !8373. Use the merged successors as authoritative design evidence and retain the closed MRs only as negative or historical context.
