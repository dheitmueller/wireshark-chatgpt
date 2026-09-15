# pcapng Extension Architecture Conventions

This file records durable pcapng/Wiretap architecture conventions extracted from upstream Wireshark merge-request history. Current upstream source remains authoritative.

## Normalize container framing before dispatching to block handlers

Common pcapng code should validate framing that is common to every block before dispatching to a block-specific handler. In particular, validate that the total block is large enough for the common header/footer and pass the handler the semantic **block content length**, not a total length that forces every handler to know and repeatedly subtract common framing.

Merged !20136, authored and merged by Guy Harris, moved the minimum-size check into common pcapng code and changed registered handlers to receive `block_type` plus `block_content_length`. It also removed now-redundant handler-specific minimum-size definitions and added additional DSB length checks.

**Implementation rule:** normalize generic framing exactly once at the common dispatch boundary. Handler APIs should expose the domain the handler actually parses rather than forcing every subtype implementation to reconstruct container bookkeeping.

**Confidence:** Very high. Merged master architecture change authored and merged by Guy Harris.

## Keep extension-private per-section state out of common structures

A format extension that needs state for the lifetime of a pcapng section should not grow extension-specific members in the generic `section_info_t`. Provide a generic keyed mechanism owned by the common layer, and let the extension allocate, retrieve, and free its private state through that mechanism.

Merged !20120, authored and merged by Guy Harris, added generic per-section associative storage keyed by PEN for custom extensions and by block type for local extensions, then moved Netflix BBLog state out of `section_info_t`. Merged !20122 continued the boundary cleanup by moving Netflix-specific definitions and data out of common Wiretap headers and structures. Merged !20132 completed the usable extension contract by permitting low-level option handlers to modify the section information they are associated with.

**Implementation rule:** common infrastructure should own lifetime and lookup mechanics; extension modules should own extension-specific representation and semantics. Prefer stable generic extension points over adding one field or API per vendor/private block type.

**Confidence:** Very high. A coherent sequence of merged master changes authored and merged by Guy Harris.

## Minimize extension API surface after genericization

When functionality becomes table-registered or otherwise internal to an extension module, remove obsolete public helpers and make implementation-only callbacks `static` rather than preserving an accidental API surface.

Merged !20131 removed an unused Netflix BBLog accessor and made registered BBLog callbacks static because they were not directly called outside their implementation file. Merged !20102 removed another unused Netflix-specific helper left behind by genericization.

**Implementation rule:** after extracting generic infrastructure, perform a second pass over the old extension API. Delete dead compatibility scaffolding unless it is intentionally supported, and give internal registration callbacks internal linkage.

**Confidence:** Very high. Merged Guy Harris-authored cleanup immediately following the genericization series.

## Use canonical alignment helpers instead of local padding formulas

For pcapng alignment, use the established `WS_ROUNDUP_n()` / `WS_PADDING_TO_n()` helpers rather than local aliases or repeated arithmetic. Compute padding once when practical and reuse that value rather than recomputing the same expression at the consumption point.

Merged !20113 and !20110, both authored and merged by Guy Harris, replaced local/hand-rolled alignment calculations with the common helpers and moved nonnegative byte counts toward unsigned types. Merged !20096 independently removed redundant SPB padding recomputation and used `WS_ROUNDUP_4()` for option sizing.

**Confidence:** Very high. Repeated merged master cleanup by Guy Harris.

## Replace vendor-specific dispatch with registered generic extension points

When common packet or file-format code has accumulated knowledge of one vendor-specific option or record family, move the semantic handling into the owning module and dispatch through a generic registration mechanism. Where practical, shape the callback like an existing Wireshark dissector interface rather than inventing a parallel callback convention.

Merged !19979, authored and merged by Guy Harris, reshaped the custom-binary-option callback to use the standard dissector signature. Merged !19980 then introduced a dissector table for custom binary options, moved Netflix Black Box Log dissection out of `packet-frame.c` into `packet-bblog.c`, and removed the last Netflix-specific knowledge from the common frame dissector. Merged !19957 independently moved Wiretap block-option registration toward module-owned registration; during review Guy Harris explicitly raised replacing record-type switch statements with lookup-table registration for non-core record types.

**Implementation rule:** common infrastructure should identify and route extension data; extension modules should interpret it. Prefer established dissector-table/registration idioms, and reserve hard-coded switches for genuinely core closed sets rather than letting them grow with every extension.

**Confidence:** Very high. Multiple merged master changes, including a coherent series authored and merged by Guy Harris plus direct Guy Harris architecture review.

## Treat extension registration as ownership of a format identifier

A registration API should prevent extensions from silently replacing handlers for identifiers already owned by core code or another extension. At the same time, policy about which identifiers may be registered should follow the external format's namespace rules rather than assuming that every non-core identifier is necessarily private/local.

Merged !19841 converted Sysdig pcapng block handling to `register_pcapng_block_type_handler()` as part of shaping a plugin-style block API. In review, Guy Harris explained that registration checks serve in part to prevent plugins from overriding block types already handled by `pcapng.c`; he also distinguished officially assigned but not-yet-supported block types from arbitrary unregistered numbers. The implementation used first-registration ownership so a later registration could not override an existing handler.

**Implementation rule:** make handler registration collision-safe and preserve a single authoritative owner for each format identifier. Keep collision prevention separate from namespace-policy validation: an officially assigned extension identifier may legitimately be implemented outside core code, while arbitrary squatting on externally governed identifiers should not be encouraged.

**Confidence:** Very high. Merged master architecture work with direct Guy Harris review.

## Preserve section-scoped identity when rewriting or merging captures

If a pcapng object is identified only within a section, references to that object cannot safely be treated as capture-global when sections are combined, reordered, or rewritten. A writer that combines sections must either preserve section boundaries or rewrite both the object identifiers and every reference to them consistently. Compatibility with external readers is part of the design constraint, not an afterthought.

Merged !19734 added support for Darwin's legacy process-information blocks and triggered an extended review of how those blocks interact with pcapng section semantics. Guy Harris clarified that stored process-information blocks, like IDBs, are section-specific. John Thacker then pointed out that Wireshark/dumpcap commonly combine sections and that IDB identifiers are rewritten in packets in some such paths; the same issue therefore has to be solved deliberately for process-information references rather than assuming their IDs remain meaningful after a merge. The discussion also explicitly considered whether rewritten files would remain readable by existing Darwin `tcpdump`, and the MR ultimately merged after months of iteration and supplied sample captures for both pktap and droptap metadata.

**Implementation rule:** whenever a block or option contains an identifier that is scoped to a section, audit every save/merge/passthrough path for that scope. Do not move definitions across sections or collapse sections unless all dependent references are remapped consistently, and validate the resulting file against important external readers when compatibility matters.

**Confidence:** Very high. Merged master feature with prolonged architecture review including direct Guy Harris and John Thacker discussion.