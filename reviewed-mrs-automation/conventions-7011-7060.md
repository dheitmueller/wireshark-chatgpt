# Durable conventions from !7011–!7060

Corpus: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Capture length semantics

Merged !7057 removes Wiretap's generic `caplen > len` clamp. Tomasz Moń showed that the clamp discarded bytes actually present in a Linux usbmon capture. Guy Harris explained the pcap/pcapng contract: captured length is the amount retained in the capture record; packet length is the unsliced logical record size. That relationship is used to distinguish capture truncation from malformed protocol data.

**Rule:** preserve captured bytes. Handle inconsistent length metadata in the format/linktype-specific layer where enough information exists to repair or explicitly diagnose it, rather than silently discarding data in generic Wiretap code.

## Per-PDU packet state

Merged !7035, authored by John Thacker, snapshots and restores TCP addresses, port type, and ports around repeated upper-layer PDU dissection. Upper-layer dissectors may legitimately rewrite packet endpoints, but those changes must not affect TCP reassembly or later sibling PDUs.

**Rule:** lower-layer code that invokes multiple sibling PDUs should restore its own packet context before each sibling and before lower-layer state operations.

Merged !7033 further shows that desegmentation offsets on a reassembled MSP are coordinates in the logical reconstructed byte stream, not the current physical TCP segment.

## Retained key lifetime and allocator ownership

Merged !7018 and !7028 fix conversation keys whose backing storage did not outlive the map/conversation retaining them. `wmem_map` stores key pointers rather than copying keys. Merged !7047 removes a mismatched `g_free()` of wmem-scope storage.

**Rule:** retained container keys need storage at least as long-lived as the entry. Match release behavior to the allocator family and scope that owns the object.

## Qt semantic values versus display values

Merged !7053 moves Traffic Tables to Qt model/view. Merged !7054 adds an unformatted role so export can use raw typed values while `Qt::DisplayRole` remains presentation-oriented.

**Rule:** sorting, filtering, export, and machine-readable output should consume semantic model values rather than reparsing pretty/localized display strings. Optional-feature-off builds should be tested after large model/view refactors; !7058 fixed the no-MaxMindDB configuration.

## Platform predicates

Merged !7051, authored by Guy Harris, gates MSVC `__cpuid` on x86/x64 architecture. Merged !7022 narrows a workaround from broad Windows gating to the MSVC toolchain.

**Rule:** OS, compiler, architecture, packaging environment, and runtime capability are separate predicates. Condition platform-specific code on the property that actually makes it valid.

## Plugin lifecycle phases

Merged !7060 adds an EPAN plugin `post_init()` callback because ordinary plugin `init()` runs before `proto_init()`, while some work requires fully initialized EPAN/protocol state.

**Rule:** expose distinct lifecycle phases when extension callbacks have different prerequisites; avoid relying on incidental startup ordering.

## Preserve parser state across byte-preserving copies

Merged !7043, authored by John Thacker, makes Save-with-Copy preserve the existing Wiretap object and only reopen the file descriptors/path. This avoids needless re-detection, preserves manually selected readers, and retains metadata state that may depend on blocks later in the file.

**Rule:** for a byte-for-byte capture copy, preserve parser state that remains semantically valid and update only changed resources.

## Safe default protocol claims

Merged !7020 disables EOBI by default because it has no heuristic and claims nonstandard high ports; its generator is updated too.

**Rule:** a dissector without a strong recognizer should not broadly claim deployment-specific/unregistered ports by default. Prefer an assigned binding, Decode As/configuration, or disabled-by-default behavior.

Merged !7045 additionally shows that new capture linktype assignments should be coordinated with the external libpcap/linktype ecosystem before merge.

## Build helper tools

Merged !7019 passes the already-resolved `CMAKE_COMMAND` into `win-setup.ps1`.

**Rule:** when the parent build has resolved a tool path, pass that exact executable to helper scripts instead of requiring another PATH lookup.

## Backportable fixes

In merged !7013, Alexis La Goutte asks that a Touchlink typo fix be separated from a feature so the fix can be backported; focused backports then appear as !7031 and !7032.

**Rule:** separate narrow correctness fixes from enhancements when they have different stable-branch eligibility.

## Translation and dependency workflows

Closed !7041 supplies workflow guidance from Roland Knall: repository translation artifacts are synchronized from the project's translation system rather than edited directly.

Closed draft !7024 is lower-weight implementation evidence, but Gerald Combs's review provides a useful dependency-major checklist spanning platform packages, setup scripts, containers/builders, supported branches, tests, and release notes.
