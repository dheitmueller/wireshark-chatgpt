# Path and Buffer Conventions

This file records durable conventions for handling externally chosen path strings and for sizing temporary text used by Wireshark frontends and build tooling.

## Do not use nominal path limits to size strings derived from user paths

A compile-time pathname macro such as `FILENAME_MAX` is not a reliable semantic bound for an arbitrary path plus prefixes, suffixes, or option syntax. If the program already owns a dynamically sized path string, construct the derived string from its actual length instead of allocating a large fixed stack buffer around a nominal platform limit.

Merged master MR !12291 replaces a `FILENAME_MAX+12` stack buffer used to form dumpcap's `printname:<path>` argument with `g_strdup_printf()`, followed by explicit `g_free()`. John Thacker approved and merged the change. The important invariant is not the particular GLib helper; it is sizing from the actual input rather than assuming a filesystem-related constant is a safe bound for a transformed string.

**Implementation rule:** when forming command-line arguments, generated paths, or decorated path strings, prefer length-aware/dynamic construction from the real inputs. If a temporary allocation is necessary, keep ownership explicit and free it after the receiving API has taken or copied the value.

## Exercise build and tooling paths containing spaces

Paths used by CMake, compiler wrappers, generated scripts, and CI are data, not shell syntax. Quote or otherwise preserve them at every boundary where a path is inserted into a command, and keep regression coverage that uses deliberately awkward valid directory names.

Merged master MR !12256, authored and merged by John Thacker, fixes CMake compiler-option construction so source/build directory values used by `-fmacro-prefix-map` remain quoted when the paths contain spaces. During review, Gerald Combs explicitly asked whether this deserved a CI test; John agreed that quoting regressions were easy to introduce and also noted a Lemon failure for paths containing `=`. Merged master MR !12266, authored by Gerald Combs, then fixes another unquoted CMake wrapper-script path and changes GitLab CI to build in a directory containing spaces and emoji. The sequence is significant: a path-quoting fix was converted into continuously exercised portability coverage after maintainer review.

**Build rule:** treat source/build/install directories as arbitrary valid paths. Do not assume they contain only ASCII characters or no whitespace. When a command-line option embeds a path rather than passing it as a standalone argv element, preserve quoting/escaping through every CMake, shell, compiler-wrapper, and generated-script boundary.

**Testing rule:** where practical, run at least one CI build from a directory containing whitespace and non-ASCII characters. This catches quoting, shell-tokenization, encoding, cache-key, and generated-script assumptions that ordinary build paths hide. When a newly fixed path syntax exposes another legal metacharacter such as `=`, consider adding that case to focused tooling tests as well.

**Confidence:** Very high. The quoting fix in !12256 was authored and merged by John Thacker, Gerald Combs explicitly requested regression coverage during review, and Gerald's later !12266 made the awkward-path case part of CI. The buffer-sizing rule is independently supported by merged master !12291.

## Compose fixed-buffer output from the current cursor and remaining capacity

When a string is assembled in several bounded formatting calls, the state for the next append is the current write position plus the number of bytes still available. Reusing the original buffer pointer or original buffer size after earlier output has already been written can overwrite prior text or let a later append overrun the end.

Merged master MR !12044, authored and approved by Guy Harris, fixes exactly this class of bug in timestamp formatting. `format_fractional_part_nsecs()` is changed to return the number of bytes formatted, analogous to other bounded formatting helpers; callers then advance their pointer and decrease the remaining capacity before each append. The same change also distinguishes byte counts from character counts and rejects an impossible nanosecond fraction of one billion or more instead of silently assuming normalized input.

**Implementation rule:** for multi-step formatting into caller-supplied fixed storage, carry `(cursor, remaining)` forward after every write. Helpers intended for such composition should expose a result that lets callers update that state without rescanning the buffer, and names/comments should say `bytes` when the quantity is a byte count rather than a character count.

**Review rule:** inspect every append after the first one. Verify that both the destination pointer and capacity describe the unconsumed suffix of the buffer, and validate domain invariants before formatting values whose textual width assumes normalized input.

**Confidence:** Extremely high. Merged master correctness/API cleanup authored and approved by Guy Harris, with the buffer-overrun mechanism and return-contract rationale stated directly in the MR.
