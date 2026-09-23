# Path and Buffer Conventions

This file records durable conventions for handling externally chosen path strings and for sizing temporary text used by Wireshark frontends and build tooling.

## Do not use nominal path limits to size strings derived from user paths

A compile-time pathname macro such as `FILENAME_MAX` is not a reliable semantic bound for an arbitrary path plus prefixes, suffixes, or option syntax. If the program already owns a dynamically sized path string, construct the derived string from its actual length instead of allocating a large fixed stack buffer around a nominal platform limit.

Merged master MR !12291 replaces a `FILENAME_MAX+12` stack buffer used to form dumpcap's `printname:<path>` argument with `g_strdup_printf()`, followed by explicit `g_free()`. John Thacker approved and merged the change. The important invariant is not the particular GLib helper; it is sizing from the actual input rather than assuming a filesystem-related constant is a safe bound for a transformed string.

**Implementation rule:** when forming command-line arguments, generated paths, or decorated path strings, prefer length-aware/dynamic construction from the real inputs. If a temporary allocation is necessary, keep ownership explicit and free it after the receiving API has taken or copied the value.

## Exercise build and tooling paths containing spaces

Paths used by CMake, compiler wrappers, generated scripts, and CI are data, not shell syntax. Quote or otherwise preserve them at every boundary where a path is inserted into a command, and keep regression coverage that uses deliberately awkward valid directory names.

Merged master MR !12266, authored by Gerald Combs, fixes an unquoted CMake wrapper-script path and changes GitLab CI to build in a directory containing spaces and emoji. The CI choice is significant: it converts a historically fragile portability case into a continuously exercised invariant rather than relying on review to notice missing quoting.

**Build rule:** treat source/build/install directories as arbitrary valid paths. Do not assume they contain only ASCII characters or no whitespace.

**Testing rule:** where practical, run at least one CI build from a directory containing whitespace and non-ASCII characters. This catches quoting, shell-tokenization, encoding, cache-key, and generated-script assumptions that ordinary build paths hide.

**Confidence:** High. Both rules are based on merged master changes; !12266 was authored by project lead Gerald Combs and !12291 was approved/merged by John Thacker.