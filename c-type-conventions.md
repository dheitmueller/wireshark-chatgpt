# Wireshark C Type Conventions

This file records durable type-usage conventions extracted from accepted Wireshark review. Current upstream source and coding guidance remain authoritative.

## Prefer standard C types in Wireshark-owned APIs and dissector code

Wireshark-owned APIs and code should not introduce or retain GLib scalar aliases where standard C types express the value domain directly. Use fixed-width types such as `uint8_t` and `uint32_t` for protocol-width or explicitly sized integer values, and use standard `bool` for Wireshark-owned boolean contracts unless an external API requires its own scalar type.

In merged MR !24689, Jaap Keuter reviewed new ENIP dissector code and explicitly requested replacing the newly introduced GLib types with `uint8_t` throughout the change, noting that Wireshark was no longer using those GLib types. Michael Mann pointed the contributor to `tools/convert-glib-types.py`; the contributor applied the review, Anders Broman approved, and the MR merged.

Merged MR !15474 provides earlier and broader evidence at a core Wireshark API boundary. Gerald Combs changed the preferences API from GLib scalar types to C99/C standard types, including `guint32` to `uint32_t` and `gboolean` to `bool`; Anders Broman approved and merged the change. This shows that the standard-type preference is not limited to dissector-local variables and can apply to Wireshark-owned interfaces themselves.

**Implementation rule:** for Wireshark-owned protocol values and APIs, prefer the project's standard C/fixed-width types rather than legacy GLib scalar aliases. Preserve dependency-defined scalar types at true external API boundaries when the called API's signature requires them; do not mechanically convert across an ABI/API contract. In particular, use `bool` for a Wireshark-owned boolean contract but keep `gboolean`/`TRUE`/`FALSE` when calling a GLib API that declares that convention.

**Confidence:** Very high. Independent merged evidence from a modern dissector review plus a core preferences-API conversion authored by Gerald Combs and approved/merged by Anders Broman.

## Do not assume the underlying C type of typedefs such as `time_t` when formatting

A typedef's concrete integer type can vary by platform. Format it through a representation whose format contract is known, or use an existing Wireshark formatting helper, rather than assuming that the typedef maps to `long`, `int`, or another particular base type.

Merged MR !23925, authored and merged by John Thacker, fixes TRDP formatting that used `%ld` for `nstime.secs`: `time_t` can be `long long` on supported platforms such as MSYS2. The accepted fix casts to `intmax_t` and uses `%ji`; John also noted that the existing `display_signed_time()` helper could be appropriate for this kind of display.

**Implementation rule:** when formatting platform-dependent typedefs, either convert to a known-width/max-width C type and use its matching format specifier or use the project's semantic formatting helper. Never infer a printf format solely from how the typedef happens to be defined on the development platform.

**Confidence:** Very high. Merged portability fix authored and merged by John Thacker.

## Do not collapse status-code return values into booleans

An API return value whose domain is an enum or status code is not a truth value merely because it is integer-valued in C. In particular, success is often represented by zero; assigning such a result directly to `bool` silently reverses the intended sense and also discards error distinctions that may matter to callers or future maintenance.

Merged MR !23846 fixes BLF interface mapping after code stored the result of `wtap_block_get_string_option_value()` directly in a `bool`. `WTAP_OPTTYPE_SUCCESS` is zero, so a successful lookup became `false`. During review Guy Harris explicitly called out the status domain and suggested preserving the `wtap_opttype_return_val` result and testing it against `WTAP_OPTTYPE_SUCCESS`; the accepted change uses an explicit `== WTAP_OPTTYPE_SUCCESS` comparison where a boolean predicate is needed.

**Implementation rule:** preserve enum/status returns in their declared semantic domain, or convert them to a predicate only with an explicit comparison to the named success/failure value. Never rely on generic C truthiness for an errno-style or enum-style status API unless that API explicitly defines boolean semantics.

**Confidence:** Extremely high. Merged master correctness fix with direct Guy Harris review identifying the semantic error and the correct comparison model.

## Size platform typedefs from the typedef itself; do not type-pun through an assumed width

A standards-defined typedef can specify a semantic category without fixing its storage width. Writing through a pointer cast to a guessed fixed-width integer is therefore not a portable way to populate such an object: on a wider implementation it may write only part of the object, and which part is overwritten becomes byte-order dependent.

Merged MR !22877, authored and merged by John Thacker, fixes `rawshark` memory-limit handling for `rlim_t`. POSIX requires `rlim_t` to be an unsigned integer type but does not require it to be 32 bits; many 32-bit systems nevertheless use a 64-bit `rlim_t`. The old code cast `&limit.rlim_cur` and `&limit.rlim_max` to `uint32_t *`, which could partially overwrite a wider `rlim_t` and behave incorrectly on big-endian systems. The accepted implementation parses into a correctly sized fixed-width temporary and then assigns the value normally to the `rlim_t` members.

**Implementation rule:** when a platform typedef has implementation-defined width, determine behavior from the typedef's actual size/contract and convert through a correctly typed temporary. Do not populate it by casting its address to an unrelated fixed-width pointer merely because that matches the typedef on the development machine; normal assignment preserves representation, aliasing, alignment, and byte-order correctness.

**Confidence:** Very high. Merged master portability correction authored and merged by John Thacker, with both the standards contract and big-endian failure mode documented in the MR.

## Match preference backing storage to the preference API contract, not an enum's implementation-defined representation

In C17, an enum's compatible integer type and therefore its signedness are implementation-defined. That makes an enum declaration a poor basis for deciding the pointee type handed to an API whose contract explicitly expects `int *` or another fixed scalar type.

Merged master MR !22453, authored by John Thacker and merged by Michael Mann, fixes ASTERIX enum preferences whose backing variables and structure members used `unsigned` even though Wireshark's enum-preference registration stores values through `int *`. John notes that the preference values fit in `int`, so the well-defined representation is to use `int` for the backing variables and pointers and perform an explicit value conversion only where an unsigned comparison is actually required.

**Implementation rule:** make callback/preference/API backing storage exactly match the API's declared pointer type. Do not try to mirror an enum's presumed signedness in storage passed through that API; under C17 the enum's representation is not a portable storage contract. Convert at semantic use sites when domains differ rather than passing an incompatible pointer or depending on implementation-defined enum representation.

**Confidence:** Very high. Merged master portability/type-correctness change authored by John Thacker and merged by Michael Mann.

## Encode fixed-size binary object contracts in the parameter type when practical

A bare `uint8_t *` says where bytes begin but does not communicate that a function requires exactly one fixed-size object. When the object size is intrinsic to the API contract, a pointer to an array can make that requirement visible to both callers and the compiler without inventing a wrapper structure.

Merged master MR !22425, authored and merged by Guy Harris, changes `str_to_eth()` from taking an unbounded byte pointer to taking `uint8_t (*)[6]`. Guy's rationale is direct: a MAC-48 address is an array of six octets, so the function signature should represent that object. Callers correspondingly pass the address of their six-byte array.

**Implementation rule:** for APIs operating on a single fixed-size binary object, consider expressing the complete object shape in the C type rather than accepting an undifferentiated byte pointer. This is particularly useful for identifiers such as fixed-width link-layer addresses where the size is part of the semantic contract. Do not force this pattern across variable-length buffers or external ABI signatures where a pointer-plus-length contract is the correct abstraction.

**Confidence:** Extremely high. Merged master API cleanup authored and merged by Guy Harris with an explicit semantic-type rationale.

## Use standard C integer format macros with standard C integer types

When modernizing Wireshark-owned integer code from GLib aliases to standard C fixed-width types, modernize the corresponding formatting contract as well. Use the `<inttypes.h>` `PRI*`/`SCN*` macros that match the standard integer type rather than retaining GLib-specific format macros.

Merged MR !16471, authored by Gerald Combs and merged by Anders Broman, explicitly replaces `G_GUINT64_FORMAT` and `G_GINT64_FORMAT` with `PRIu64` and `PRId64` and recommends the C99 forms. The surrounding merged conversion series (!16449-!16457, !16464-!16468, and !16483-!16487) systematically moves dissector-internal types and tvbuff accessors toward standard C forms, making !16471 an explicit formatting rule within a broad accepted migration rather than an isolated cleanup.

**Implementation rule:** after choosing a standard fixed-width integer type for Wireshark-owned data, use the matching standard format macro for printf/scanf-family formatting. Keep type and format contract paired; do not rely on platform-specific assumptions about the underlying base type or mix a migrated C type with an unrelated library-specific format macro.

**Confidence:** Very high. Explicit merged master recommendation backed by a broad merged type-conversion series.

## Bulk type conversion must preserve dependency API boolean conventions

Type-modernization scripts are useful for large mechanical migrations, but they must distinguish Wireshark-owned values from dependency API parameters. In particular, GLib functions declared with `gboolean` parameters should continue to use the GLib boolean convention expected at that API boundary rather than being rewritten blindly to C `bool` literals.

During review of merged !16450, Stig Bjørlykke explicitly requested retaining `gboolean` semantics for GLib APIs and stated that GLib functions using `gboolean` should keep `TRUE`/`FALSE`. The conversion was corrected afterward: merged !16458 restores `TRUE` in `g_string_free()` calls in `packet-f5ethtrailer.c`, and merged !16459 does the same in `packet-btatt.c`. This independently corroborates the notebook's existing dependency-boundary rule and shows that bulk conversion tooling needs API-aware exclusions or post-conversion review.

**Implementation rule:** do not treat `gboolean`/`bool` or `TRUE`/`true` as mechanically interchangeable across dependency calls. Conversion scripts should exclude or specially handle dependency-owned API arguments, and reviewers should check the callee prototype when a bulk migration changes scalar types or boolean spellings at a library boundary.

**Confidence:** Very high. Direct maintainer review plus two merged corrective follow-ups, consistent with independently reviewed later API-boundary evidence.
