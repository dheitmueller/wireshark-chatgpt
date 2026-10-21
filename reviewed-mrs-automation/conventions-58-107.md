# Durable conventions extracted from !58–!107

Current upstream source and later accepted review history remain authoritative.

## Windows text encodings are separate runtime domains

Merged master !91, authored and merged by Guy Harris, establishes that after Wireshark sets the Visual Studio C runtime locale to UTF-8, CRT strings such as `_tzname` are already UTF-8 and must not be converted as though they used the Windows system ANSI code page. Guy explicitly warns that this does **not** make Windows ANSI APIs or console byte I/O UTF-8. !103 immediately fixes !91's Windows build error by returning `_tzname` directly, so !103 is the final code state while !91 carries the architectural rationale.

**Rule:** identify which Windows text domain produced a string—CRT locale, UTF-16 Win32 API, ANSI code page, or console—and convert according to that domain. A UTF-8 CRT locale is not a process-wide switch for every Windows text API.

## Persisted preference keys are compatibility identifiers

Merged master !87 fixes user-visible spelling but deliberately leaves the misspelled preference key `st_sort_casesensitve` unchanged because renaming it would discard an existing saved preference.

**Rule:** preference keys are persisted identifiers, not labels. Correct display text freely, but migrate or alias a persisted key if its name must change; do not silently rename it and lose user state.

The large spelling cleanups !94, !79, !75, !74, !71, and !63 include historical filter-abbreviation renames. Those early accepted changes do not override later, stronger notebook evidence that compatibility-facing display-filter identifiers should not be renamed gratuitously.

## Respect allocator ownership

Merged master !96 removes a stale `g_free()` for a string allocated from `wmem_packet_scope()`; stable backports !98–!100 carry the same correction.

**Rule:** free memory through the allocator family that owns it. Packet/file-scope wmem allocations are reclaimed by their scope; do not individually `g_free()` them on an error path.

## Validate ranges before subtraction and allocation

Merged master !80 rejects `usage_min >= usage_max` before computing `usage_max - usage_min` and passing it to `wmem_array_grow()`.

**Rule:** validate ordering and bounds before unsigned subtraction, allocation growth, loop counts, or pointer arithmetic.

## Registered field metadata constrains programmatic values

Merged master !84, authored by Pascal Quantin, replaces an arbitrary checksum status value with `PROTO_CHECKSUM_E_BAD`. The arbitrary value had no representation for the `BASE_NONE` status field and could assert during column rendering. Stable backports !86, !88, and !102 corroborate the fix.

**Rule:** a generated/programmatic value added to a registered field must lie in that field's declared semantic/table domain; numeric storage does not imply every integer is representable.

## Decode displayed fields once when parser logic also needs the value

In merged master !73, Anders Broman explicitly recommends `proto_tree_add_item_ret_uint()` / `_int()` instead of separately fetching a value from the TVB and adding the same bytes to the tree. Alexis La Goutte separately asks that masks use registered field/bitmask semantics. The accepted revision follows both.

## Fix generators at the source and regenerate output

Merged master !60 fixes Python `lstrip('nl80211_')`, which strips a set of leading characters rather than one exact prefix. The accepted change implements exact-prefix removal in the generator and regenerates the affected nl80211 fields. !81 similarly changes the NGAP ASN.1 template and generated dissector together.

**Rule:** repair generator/template semantics at the source and commit synchronized generated output.

## Prefer corroborating structural evidence in heuristics

Merged master !90, authored by John Thacker, infers FCoE Ethernet FCS presence by validating the protocol EOF plus padding at candidate positions, not from a single weak signature.

## Shallow copies can duplicate ownership

Merged master !85 fixes a USB HID double free by allocating a fresh `field.usages` array after appending a field record.

**Rule:** after storing a structure whose pointer members represent per-record ownership, replace/reinitialize those owned subobjects before reusing the working structure.

## Cross-platform builds constrain neutral refactors

Merged !58 consolidates duplicate CIP connection state but triggers a macOS clang error on aggregate `{0}` initialization. Merged corrective !65 replaces those initializers with explicit `memset`.

**Rule:** even behavior-neutral refactors must pass the complete supported compiler/build matrix; immediate corrective follow-ups are part of the accepted final shape.

## Submission workflow is part of reviewability

Merged !107 makes maintainer-edit permission a CI-validated MR requirement; !101 shows Pascal Quantin applying it. In !64 Pascal asks for a rebase rather than a merge commit and a component-prefixed subject. !88 records Guy Harris questioning a misleading title after two commits were accidentally pushed; Gerald Combs corrects it. !95 / superseded !93 document rebase recovery.

**Rule:** keep topic history linear and focused, use the expected component-prefixed subject, and permit maintainer edits where required.

## Closed/superseded evidence

!104 has no accepted patch. !97 is an unfinished EPL change. !93 is superseded by !95. !92 is superseded by the later accepted PROFINET multi-AR work in !272. !76 is superseded by !77. !72 was abandoned for a fresh rebased MQ submission. !66 was overtaken by newer SSH decryption work; John Thacker explicitly recommended a new MR. !61 is an abandoned automatic-update draft.
