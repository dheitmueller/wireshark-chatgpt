# Durable convention synthesis — Wireshark MRs !2161–!2210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Runtime file-format identities belong to the Wiretap registry

Guy Harris's merged !2164 and !2201 remove additional fixed `WTAP_FILE_TYPE_SUBTYPE_*` constants. File handlers register themselves and receive runtime subtype identities; well-known formats expose accessors when callers need a stable semantic query. Compatibility names remain registration policy rather than a reason to preserve permanent numeric identities.

The initialization consequence is explicit in !2164: `wtap_init()` must run before libwireshark/dissector registration when dissector handoff needs a file-type/subtype value.

**Rule:** treat registered numeric IDs as runtime registry results. Initialize the provider registry before consumers resolve/bind those IDs, and expose semantic accessors where a caller needs a particular well-known format.

## File handlers should advertise exact abstract capabilities

Guy-authored !2183 replaces coarse booleans such as name-resolution/comment/interface support with per-format block and option tables that also record multiplicity. Wiretap's abstract data model need not match literal on-disk block structure; a reader/writer may synthesize the abstract block. `WTAP_BLOCK_IF_ID_AND_INFO` is only appropriate when packet records can actually be associated with an interface.

Guy-authored !2192 immediately fixes a nested table-walk bug from this new machinery, replacing ambiguous `i`/`j` handling with separate block and option indices.

**Rule:** make each format owner declare the precise semantic structures it supports and let generic code query that contract. In nested tables, name/index each semantic domain independently.

## Let the registered field decode the value once

In merged !2195 Anders Broman recommends `proto_tree_add_item_ret_uint()` with the correct endian. Because the `hf_` registration already carries the mask, the helper returns the masked logical value; a second manual mask/shift is unnecessary. The output object is changed to `guint32`, matching the helper API.

**Rule:** when the registered field metadata already expresses the wire mask and byte order, use the matching return-value helper and consume its decoded result. Do not duplicate the extraction transform, and do not cast a narrower destination to satisfy a wider output contract.

## Preserve indexed lookup invariants

In merged !2188 Anders Broman catches an SCTP PPID `value_string_ext` entry whose numerical order regressed. `tshark -G` reports that the extension table fell back to linear search. Filling unassigned values and restoring ordering makes indexed lookup possible again.

**Rule:** treat sortedness as part of the `value_string_ext` performance contract and pay attention to glossary/startup warnings that announce fallback.

## Recursion suppression comes after bounding analysis

In merged !2184 Gerald Combs explains that clang-tidy's recursion warning is intentional. Potentially unbounded dissector recursion should be guarded with the dissection-depth helpers. A narrow `NOLINTNEXTLINE(misc-no-recursion)` is appropriate only after the path has been made/proven safe.

**Rule:** the goal is bounded stack/work on malformed captures, not a quiet analyzer.

## Generated files depend on their real generator inputs

Guy-authored !2187 fixes the custom command for `wtap_modules.c` to depend on `WIRETAP_MODULE_FILES`, the files actually scanned by the generator, instead of an adjacent but semantically wrong source list.

**Rule:** CMake custom-command dependencies should model the generator's true inputs so regeneration occurs exactly when its input set changes.

## Prefer visible standard dissector mechanics

Anders Broman's review of merged !2178 explicitly rejects a private macro layer for ordinary field addition because it obscured lengths and offset advancement. The accepted code returns to ordinary `proto_tree_add_item()` calls plus explicit offset movement, which aligns with surrounding dissectors and is easier to audit. Windows CI also found nonportable integer types/narrowing.

**Rule:** abstract genuinely repeated semantics, not routine field mechanics merely to reduce line count. Keep wire lengths and parser progression visible, and use project-portable types.

## Create shared headers only for actual sharing

In merged !2161 Pascal Quantin asks whether DCCP service codes need to be consumed outside the DCCP dissector. Wireshark prefers to avoid new headers when the definitions are private. The header becomes justified only after a real cross-dissector use case is stated, and it is then added to the epan build/public-header list.

**Rule:** headers are interfaces. Keep protocol constants local until another translation unit has a genuine consumer; once shared, wire the interface into the build/export surface deliberately.

## Standardize capture-link identity instead of abusing USER DLT

Closed !2168 is lower-weight, but Guy Harris's discussion is highly authoritative and later merged !2599 corroborates it. If an extcap source carries 802.11 plus radio metadata, use an established metadata LINKTYPE such as radiotap/AVS or obtain a LINKTYPE assignment rather than asking users to configure a USER DLT merely to reach an existing dissector. The metadata dissector should fully initialize the downstream pseudo-header it owns.
