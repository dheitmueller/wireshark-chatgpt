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