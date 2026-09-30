# Durable Wireshark conventions from MRs 4161–4210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Explicit user dispatch choices outrank default transport heuristics

Merged master MR !4206, authored by John Thacker, changes TCP, UDP, and SCTP dispatch so a table entry changed by Decode As or a registered preference is tried before the ordinary lower-port/default ordering. The implementation adds APIs that can distinguish a user-modified registration from the table default.

**Rule:** when the user has explicitly overridden a dissector-table binding, honor that explicit choice before applying fallback heuristics such as well-known/lower-port preference. Preserve the normal heuristic ordering only when both candidates remain at their defaults.

**Confidence:** Very high. Merged cross-transport framework change authored by John Thacker.

## Generated dissector fixes belong in the authoritative template

Merged master MR !4202, authored by Guy Harris, fixes IEC 61850 SV trailer handling by changing the ASN.1 template and regenerating the C dissector. Guy explicitly states that the semantic fix belongs in the template so regeneration cannot overwrite it.

**Rule:** edit the generator/template/conformance source that owns generated dissector behavior, then regenerate the derivative. Do not place a semantic fix only in generated C.

The same MR uses `set_actual_length()` for a protocol carried directly over Ethernet so bytes beyond the protocol-declared extent remain available to the parent Ethernet dissector as trailers/FCS.

**Boundary rule:** when a child protocol has an authoritative declared body length but its parent owns possible trailing bytes, bound the child TVB to the child extent rather than consuming the parent's trailer.

**Confidence:** Extremely high. Direct merged Guy Harris implementation.

## Propagate bit-order semantics through every API layer

Merged master MR !4200 extends little-endian bit numbering through `tvb_get_bits*`, `tvb_get_bits_array()`, proto-tree bit helpers, and display formatting. Existing callers were made explicitly `ENC_BIG_ENDIAN` to preserve behavior while USB HID opted into `ENC_LITTLE_ENDIAN`. Capture testing then exposed one remaining hidden big-endian assumption in the FT_BYTES path, which was fixed and retested.

**Rule:** if an encoding/bit-order parameter affects the meaning of a field, carry it through extraction, array conversion, formatting, and tree presentation rather than fixing only the leaf caller. When evolving the API, make old semantics explicit at existing call sites.

**Testing rule:** validate with captures whose values distinguish the competing bit orders; symmetric or byte-aligned test values are insufficient.

**Confidence:** Very high. Merged framework/API change with reviewer capture validation.

## Give parser/helper objects explicit allocator scope

Merged MRs !4193 and !4194, authored by Evan Huus, make tvbparse and ptvcursor take an explicit `wmem_allocator_t *`. The parser stores the scope and uses it for all derived tokens/strings/stacks. The cursor cannot reliably infer lifetime from its tree because the tree may be NULL or replaced. Merged !4185 and !4186 extend the same direction to WSLua and OSI helpers.

**Rule:** allocation lifetime is part of a reusable helper's API contract. Pass or retain an explicit allocator when presentation objects or global packet scope do not prove the correct lifetime. Prefer scopes the compiler and caller can verify, commonly `pinfo->pool` during dissection.

**Confidence:** Very high. Coherent merged series by Evan Huus.

## Handoff callbacks must separate one-time registration from repeatable preference application

Merged master MR !4208 moves invariant Infiniband registration, handle creation, Decode As registration, and heuristic installation into an initialization-only block while leaving the preference-selected RRoCE UDP binding repeatable and removing the old binding on later calls.

**Rule:** design `proto_reg_handoff_*` as potentially re-entered by preference changes. Register invariant framework objects once; repeat only state that actually derives from mutable preferences, undoing the previous preference-selected binding first.

**Confidence:** Very high. Merged master implementation; corroborates later reviewed !4400.

## Wiretap EOF meaning depends on record position

Merged master MR !4168, authored by Guy Harris, makes the distinction explicit in BLF: EOF on the first read that attempts to start the next packet can mean normal end of capture; EOF on any subsequent read after a record has begun means the file is truncated and becomes `WTAP_ERR_SHORT_READ`.

**Rule:** normal EOF is a record-boundary condition. Once a packet/record has been recognized and parsing has started, failure to obtain required bytes is a short-read/error, not clean end-of-file.

**Confidence:** Extremely high. Direct Guy Harris implementation.

## Wiretap failure paths must classify errors, not merely return FALSE

Merged master MR !4169, also authored by Guy Harris, adds explicit `WTAP_ERR_BAD_FILE`, `WTAP_ERR_UNSUPPORTED`, and `WTAP_ERR_INTERNAL` reporting plus owned `err_info` text to BLF failure paths that previously only logged debug text.

**Rule:** a Wiretap helper returning failure must leave the caller enough information to distinguish malformed input, unsupported format features, internal invariants, normal EOF, and short read. Do not let a failure with `err == 0` accidentally masquerade as EOF.

**Confidence:** Extremely high. Direct Guy Harris implementation.

## Weak heuristic recognition should remain opt-in

Merged master MR !4176, authored by John Thacker, disables VSS port-stamp-only recognition by default because essentially arbitrary short trailers can match it, while retaining stronger timestamp-backed recognition as the default path.

**Rule:** default-enable a heuristic only when the recognizer has enough independent evidence to keep false positives acceptably low. Preserve weaker recognition behind an explicit preference when it is useful but inherently ambiguous.

**Confidence:** Very high. Merged master implementation; later stable/related review already reinforced this convention.

## Optional code with no build path will rot

During merged !4170, Roland Knall required the contributor to split an unrelated compile-disabled experimental mode from a legitimate Qt refactor. He specifically warned that code hidden behind a private compile define will stop building unnoticed; a supported CMake option would at least make automated build coverage possible. The final MR removed the experimental part.

**Rule:** do not bury unrelated experimental behavior in an otherwise reviewable refactor. If optional code is worth retaining, give it a supported build switch and deliberate CI coverage; otherwise keep it out of the merged tree.

**Confidence:** High. Direct maintainer review incorporated before merge.

## Generic formatting belongs in the lowest common owning library

Merged !4210 and !4196 continue moving generic numerical/string formatting from epan into wsutil, with corresponding symbol-manifest and test updates.

**Rule:** generic utilities that do not depend on packet-analysis semantics belong in the lower reusable library, and moving a public helper across libraries requires synchronized ABI/export metadata plus tests.

**Confidence:** Very high. Merged master series authored by João Valverde.

## Superseded code can still carry review lessons, but not implementation precedent

Closed !4209 contains useful review on using existing `wscbor` facilities, avoiding direct stderr output, running `check_static.py`, preserving LGPL source provenance, and maintaining protocol-layer separation; it was superseded by later merged !4497. Closed !4192 and !4179 were likewise overtaken by cleaner or narrower successors.

**Review rule:** retain durable reviewer guidance from abandoned work, but treat the merged successor as the implementation authority whenever one exists.
