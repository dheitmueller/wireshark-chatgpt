# Wireshark Filesystem Path Conventions

This file records durable conventions for manipulating filesystem pathnames portably. Current upstream platform and utility APIs remain authoritative.

## Use path-aware helpers instead of open-coding separator searches

Portable path manipulation should use the platform/library pathname helpers rather than finding the last directory component with `strrchr()` on one separator character. On Windows, callers can supply pathnames containing either accepted separator form, so code that searches only `G_DIR_SEPARATOR` can split a valid pathname at the wrong place or fail to split it at all.

Merged master MR !14155, authored by John Thacker and merged by Anders Broman, fixes ring-buffer filename handling by replacing hand-written last-separator logic with `g_path_get_basename()`, `g_path_get_dirname()`, and `g_build_filename()`. The accepted change explicitly notes that Windows accepts multiple separator forms and that using the pathname API avoids having each caller reproduce those rules. Release backports !14161 and !14162 carry the same correction to maintained branches.

**Implementation rule:** when code needs a basename, dirname, path join, or equivalent pathname operation, prefer the shared path API that knows the target platform's accepted syntax. Do not assume one separator character is a complete pathname grammar.

**Testing rule:** pathname code that must run on Windows should include mixed/alternate valid separator forms as well as ordinary native paths, particularly when deriving suffixes or filename components used for output.

**Confidence:** Very high. Merged master portability fix authored by John Thacker, accepted by Anders Broman, and propagated to two stable branches.

## Resolve resource paths from the actual packaging mode, and apply safe explicit overrides before platform defaults

The target operating system does not by itself determine how Wireshark was packaged. A macOS build can run from an application bundle or from an ordinary build/install tree, and resource lookup should test the layout that was actually enabled rather than treating `__APPLE__` as synonymous with bundle semantics.

Merged master MR !9621, authored and merged by Gerald Combs, changes bundle-specific filesystem logic to key off `ENABLE_APPLICATION_BUNDLE` instead of `__APPLE__`. The same change also makes the namespace-specific `DATA_DIR` and `PLUGIN_DIR` environment overrides take precedence consistently before Windows, build-directory, bundle, or installed-prefix path selection, while still refusing those overrides when the process started with special privileges.

**Path rule:** condition runtime resource-layout logic on the build/package mode that creates that layout, not on a broader OS macro. Apply explicit user/environment path overrides consistently across supported platforms before derived defaults, subject to the privilege/security rules that make an override unsafe.

**Confidence:** Extremely high. Merged filesystem/runtime cleanup authored and merged by project lead Gerald Combs.
