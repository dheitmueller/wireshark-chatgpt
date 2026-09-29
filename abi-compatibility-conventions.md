# Wireshark ABI Compatibility Conventions

This file records durable ABI-compatibility conventions extracted from accepted upstream Wireshark changes. Current upstream source, symbol-versioning policy, and maintained-branch requirements remain authoritative.

## Preserve exported symbols for the lifetime of a stable-branch ABI

Removing an implementation from active use does not automatically make its exported symbol safe to remove from a maintained release branch. If an already-released public symbol is part of that branch's ABI, keep the symbol available until the compatibility boundary permits its removal, even when the implementation can only be retained as a compatibility stub.

Merged release-4.4 MR !23538, authored by Gerald Combs and approved by John Thacker, restores `ws_base32_decode()` after its removal broke ABI compatibility. The accepted fix restores both the public declaration and a stub implementation specifically so binaries built against the stable branch continue to resolve the symbol.

**Implementation rule:** before deleting or renaming a public/exported function on a maintained branch, check the branch's ABI contract rather than reasoning only from source-tree call sites. If compatibility requires the symbol, retain an ABI-compatible declaration and implementation/stub until the next allowed ABI break; remove it only at an intentional compatibility boundary.

**Confidence:** Very high. Merged supported-branch ABI repair authored by Gerald Combs and approved by John Thacker, with the compatibility failure stated explicitly in the MR.

## Adding read-only `const` qualification is not inherently an ABI break

Do not treat every source-level type qualifier change on an exported C function as though it changes the binary calling convention. When a pointer argument is semantically read-only, adding `const` to the pointed-to type can make the public declaration accurately express the implementation's contract without changing how the pointer is passed at the ABI level.

Merged master MR !14257 const-ifies generated registration tables and changes exported `register_all_tap_listeners(tap_reg_t *)` to `register_all_tap_listeners(tap_reg_t const *)`. The author explicitly raised concern about modifying a `WS_DLL_PUBLIC` signature. Guy Harris replied that he knew of nothing that would cause API or ABI breakage from adding a `const` qualifier to an argument or to the target of a pointer argument, noted that callers cannot legitimately depend on a routine modifying data it now promises not to modify, and then approved the MR. He also cautioned that Wireshark does not promise strong API/ABI compatibility between major releases.

**Review rule:** distinguish binary ABI, source/API typing, and semantic mutation contracts. For a public pointer parameter that the callee does not modify, adding pointee `const` is generally compatible with the C calling convention and improves the source contract; nevertheless, review unusual uses such as function-pointer type matching and the compatibility policy of the branch being changed rather than applying a blanket rule to every qualifier edit.

**Confidence:** Extremely high for the stated Wireshark review precedent. The compatibility question was answered directly by Guy Harris on a merged MR and followed by his explicit approval.

## Package symbol metadata must match the actual exported API exactly

Distribution ABI metadata is part of the public-library contract. A symbol file that names a function incorrectly can cause package ABI checks to validate the wrong interface, even when the library itself exports the correct function. Names, suffixes, and version-introduction annotations should be derived from the actual public exports rather than reconstructed from memory or a nearby API family.

Merged release-4.0 MR !13619 and release-4.2 MR !13620 correct Debian `libwiretap` symbol files after newly added option getter/setter functions were recorded without their real `_value` suffix. The code exported names such as `wtap_block_get_int32_option_value`, while the package metadata had listed `wtap_block_get_int32_option`.

**Packaging/ABI rule:** whenever public functions are added, renamed, or backported, compare package symbol manifests against the compiled/exported API names and record the correct first-supported version. Treat the symbols file as machine-readable ABI metadata, not as approximate documentation.

**Review rule:** a public-API change is incomplete until downstream symbol/version manifests for maintained release branches agree with the real binary exports. Small naming differences such as suffixes are correctness issues because packaging tools consume the strings literally.

**Confidence:** Very high. Two merged maintained-branch fixes correct concrete symbol-name mismatches in Debian's ABI metadata and were accepted by project maintainers.


## When an exported API moves between libraries, move the package symbol metadata with it

A source-level refactor that relocates a public function from one shared library to another changes which binary owns that exported symbol. Distribution symbol manifests must remove the symbol from the old library and add it to the new library with the correct first-version annotation; otherwise package ABI checks describe an interface that no longer matches the binaries.

Merged MR !8308, authored and merged by João Valverde, updates Debian symbol manifests after the `format_text*` helpers moved from libwireshark to libwsutil. The change removes the symbols from the former library, adds them to the latter, and normalizes the introduction version for the newly exported wsutil entries. Merged !8277 separately repairs a missing symbol-manifest entry, while !8307 leaves a source comment reminding maintainers that adding shared `true_false_string` definitions requires corresponding declarations and symbol metadata.

**Packaging/ABI rule:** whenever a public symbol is added, removed, renamed, or moved across shared libraries, audit the package symbol manifests for every affected library and record the actual exporting library and supported-version boundary.

**Confidence:** Very high. Merged package/ABI corrections by João Valverde, with the library move and symbol-manifest consequences directly visible in the accepted changes.


## New public constants/utility symbols need package-manifest entries too

The package symbol file is not limited to major APIs. Adding an exported shared constant or utility helper still changes the shared-library symbol set and requires the corresponding Debian symbols manifest entry.

Merged master MR !8208 adds the missing `tfs_not_restricted_restricted` entry after the symbol was introduced in !8206. Merged !8162 similarly adds the missing `wmem_tree_contains32` entry to libwsutil's symbols file.

**Packaging/ABI rule:** whenever `WS_DLL_PUBLIC` data or functions are added, check the symbol manifest for the library that actually exports them; small constants/helpers are ABI entries just as functions are.

**Confidence:** High. Two merged manifest follow-ups corroborating the broader symbol-metadata rule already recorded above.


## Restore accidentally removed stable-branch symbols with a compatibility wrapper and accurate provenance

If a public symbol shipped in a maintained ABI and is accidentally removed in a patch release, restoring it as a deprecated compatibility wrapper can be safer than forcing downstream binaries to absorb the removal. The package symbol manifest must record the version in which the symbol is actually available again, not pretend that an intervening release exported it.

Merged release-3.6 MR !5958, authored by Gerald Combs, restores `ws_log_default_writer()` after it disappeared in 3.6.1. The implementation is a deprecated wrapper around `ws_log_console_writer()`; the compatible library VERSION is incremented while the SOVERSION remains 13. Balint Reczey specifically requested that Debian's symbols file mark the restored symbol as first appearing in 3.6.2 because 3.6.1 did not contain it.

**Stable-ABI rule:** when repairing an accidental stable-branch symbol removal, restore link compatibility with the narrowest compatible implementation, mark the old API deprecated when appropriate, and make symbol-version metadata tell the truth about the release history.

**Confidence:** Extremely high. Merged maintained-branch ABI repair authored by Gerald Combs with explicit package-version review from Balint Reczey.

## Moving a public implementation can require updating the transitive link interface for external consumers

Merged master MR !5937 fixes external plugin linking after `wmem_alloc()` moved from libwireshark to libwsutil by adding `-lwsutil` to `wireshark.pc`. Source relocation inside the Wireshark tree does not make the new library dependency invisible to third-party consumers that link using pkg-config metadata.

**Packaging rule:** when an exported API used by external consumers moves between shared libraries, audit not only symbol manifests but also pkg-config/import/link metadata. Consumers should receive every library required to resolve the public interface advertised by that metadata.

**Confidence:** High. Merged fix for a concrete external-plugin link regression; closed predecessor !5935 was superseded only because its source branch prevented maintainer collaboration.


## Keep compatibility helpers private when the symbol belongs to a dependency

Merged master MR !4569 changes Wireshark's fallback implementation of `g_memdup2` from a compiled wsutil function into a `static inline` header definition. The purpose is to avoid exporting a symbol that is owned by GLib. Release-3.6 MR !4579 carries the same correction.

**ABI rule:** when Wireshark provides a local fallback for a newer dependency function, keep that helper private unless Wireshark intentionally defines its own public API. Otherwise a compatibility helper can accidentally expand the shared-library ABI and collide with the dependency's real symbol.

**Confidence:** Very high. Merged master ABI correction with a maintained-branch backport.
