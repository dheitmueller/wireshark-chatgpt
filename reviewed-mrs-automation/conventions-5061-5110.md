# Wireshark conventions extracted from MRs 5061-5110

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly !5110 through !5061. Merged master changes are primary evidence, release backports are corroborating evidence, and closed proposals are lower-weight history.

## Keep the generator with committed generated dissectors

Merged master !5110 adds generated ETI, XTI, and EOBI dissectors. Anders Broman asked that the Python generator be included under `tools/` so future regeneration remains maintainable, and the accepted revision does so.

**Rule:** when generated dissector source is committed, keep the reproducible generator path in the repository when practical.

**Confidence:** Very high.

## External plugin examples must use the public build boundary

Merged master !5063, authored by Guy Harris, removes Wireshark's generated `config.h` from the example plugin because that header is not available to third-party plugins. Release-3.6 !5064 carries the same correction. Earlier !5061 is therefore not authoritative for the external-plugin example.

**Rule:** examples intended to compile out of tree must depend only on the public plugin and build contract, not Wireshark-private generated build headers.

**Confidence:** Extremely high.

## Prefer `proto_tree_add_bitmask()` for packed component flags

In merged master !5079, Jaap Keuter asked that BBLog flag groups use `proto_tree_add_bitmask()`. The accepted revision replaces manually constructed parent/subtree/child groups with the bitmask helper. Stable !5108 corroborates the implementation.

**Rule:** when a fixed-width wire integer is represented as a parent plus registered masked subfields, prefer the bitmask tree helper unless the layout requires custom handling.

**Confidence:** Very high.

## Gate native-handle cleanup on successful acquisition

Merged master !5065 is the master origin of the MaxMind resolver fix later backported as !5133. A failed child-process spawn could leave a zero-initialized descriptor field equal to fd 0, so cleanup before verifying success could close standard input.

**Rule:** zero initialization is not a universal invalid-handle sentinel. Release only handles known to have been acquired, or use an API-defined invalid sentinel.

Gerald Combs also notes in !5065 that fixes normally land on master first and are then backported where needed.

**Confidence:** Extremely high.

## Treat `CMAKE_BUILD_TYPE` as single-config state

Merged master !5090 notes that `CMAKE_BUILD_TYPE` is ignored by multi-config generators. Graham Bloice points out that Visual Studio chooses configuration at build time. Later reviewed !5327 and !5344 provide stronger operational corroboration.

**Rule:** configuration-sensitive behavior that must work with Visual Studio, Xcode, or another multi-config generator must use CMake's configuration-aware mechanisms rather than assuming a generation-time build type.

**Confidence:** Very high.

## Keep unrelated contribution work separate

In merged !5109, Uli Heilmeier asks the contributor to separate unrelated commits into different merge requests and to include the issue reference in the bug-fix commit message. The contributor restructures the work accordingly.

**Rule:** keep unrelated changes in separate review units, and make bug-fix history traceable to the relevant issue.

**Confidence:** High.

## Audit producers when a user-facing grammar changes

Merged master !5105 fixes the VoIP dialog's generated display filter after set syntax changed to require comma-separated elements; !5106 backports it.

**Rule:** when parser grammar changes, audit code that emits the language as well as the parser, tests, and documentation.

**Confidence:** Medium-high.

## Supersession notes

- !5109's workflow-local test-environment workaround is superseded architecturally by the later shared-fixture solution in !5129, !5134, and !5152.
- Closed !5094 is design-history only; later accepted sequence-number guidance carries greater weight.
- !5085 should not override later !5514 and !5520 at Windows API boundaries, where the exact native return type matters.
- !5061 is superseded for the external plugin example by later !5063 and !5064.
