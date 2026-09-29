# Durable conventions from Wireshark MRs 5161-5210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly !5210 through !5161. Merged master changes are primary evidence; maintained-branch copies corroborate them. Closed !5165 and !5164 are lower-weight review history only.

## Preference callbacks and registration side effects

Merged master !5209, authored by Pascal Quantin, removes `proto_reg_handoff_websocket` as the WebSocket preferences apply callback. None of the preferences requires handoff work, and rerunning that callback attempted to register WebSocket in the TCP table again. Release-3.6 !5218, reviewed in the preceding run, corroborates the same fix.

**Rule:** register an apply/handoff callback only for work whose state actually depends on the changed preferences. A generic “rerun handoff” response is unsafe when handoff performs global registration that is not explicitly replaceable/idempotent.

## Binding-visible constants need an explicit ABI surface

Merged master !5208, authored by João Valverde, adds `epan_inspect_enums()` because C ABI metadata does not expose enum members and integer macros needed by non-C bindings. A generated C table becomes the stable runtime surface, while the pyclibrary generator is an explicit non-default target because it is comparatively slow and dependency-sensitive.

**Rule:** if supported bindings need semantic constants that the native ABI does not expose, provide a deliberate runtime/introspection contract rather than forcing consumers to scrape project headers. Heavy or fragile generation used to maintain that contract need not run in every normal build if the reviewed generated artifact is checked in.

## Wrapping sequence spaces require modular comparison—and sometimes a hard traversal bound

Merged master !5190, authored by John Thacker, fixes an RTMPT infinite loop by replacing a raw numeric TCP-sequence comparison with `GE_SEQ`, which understands wraparound. Later merged !5225 shows the second half of the problem: a wrap-aware comparison is still insufficient when the lookup container itself is ordered as ordinary 32-bit integers, so traversal also needs a conservative stop condition.

**Rule:** never use ordinary relational operators to establish ordering in a wrapping sequence-number space. Then separately inspect the data structure used to traverse that space; if its ordering is linear rather than modular, bound the walk so wraparound cannot create a cycle or revisit path.

## Validate and normalize display-filter literals in the scanner

Merged master !5182 separates character constants from generic unparsed/string nodes. !5187, authored by João Valverde, parses them in the lexer into their semantic numeric value, avoiding surprising later coercions. !5180 makes unknown double-quoted escape sequences syntax errors in the scanner, updates the User's Guide and release notes, and adds regression coverage. !5192 additionally frees the partially built scanner string before returning `SCAN_FAILED`.

**Language rule:** syntax-defined literal categories and escape validity belong at the lexical boundary. Do not pass a syntactically invalid escape downstream as ordinary text and hope type conversion catches it later; normalize a valid literal once into its semantic token representation.

**Ownership/testing rule:** scanner failure owns and cleans any partially built token state. User-visible syntax changes should carry both regression tests and release/user documentation, especially when previously accepted text becomes invalid.

## Presentation compaction must preserve filter/search semantics

Merged master !5176 makes multi-line packet comments compact in the protocol tree but adds the complete unsplit comment as a hidden item so searches and filters keep seeing the canonical value.

**Rule:** when UI presentation truncates, splits, summarizes, or otherwise transforms a packet-derived value, preserve the complete semantic value through a filterable field or equivalent internal representation when users previously relied on it. Presentation cleanup should not silently change query behavior.

## Packet visited state is not proof that this dissector's state exists

Maintained-branch merged !5162 and !5161 change Gryphon to look up its packet proto-data first and create it when absent rather than branching solely on `pinfo->fd->visited`. Strange TCP sequence behavior can cause a frame to be marked visited even though this dissector did not receive it on the nominal first pass.

**State rule:** before consuming dissector-owned per-packet state, test for the actual state object. Treat `visited` as a dissection-pass property, not as a guarantee that every nested dissector executed earlier and initialized its own bookkeeping.

## Validate vendor-specific dissector fixes with publishable real traffic

Merged master !5201 corrects Cisco ERSPAN marker layout using vendor documentation plus captures from Nexus 9000 software versions 9.2 and 10.2. Alexis La Goutte explicitly asks about compatibility with other captures. Jaap Keuter rejects a field rename whose theoretical time basis did not match the protocol/documentation semantics and asks the contributor to publish sample captures; the contributor produces anonymized samples.

**Review/submission rule:** for proprietary or thinly documented wire formats, pair reverse-engineered/specification reasoning with representative real-device captures when possible, ideally spanning relevant software versions. Sanitize captures before publication, and keep user-facing field names grounded in the protocol/documented semantic value rather than implementation theory.

## Error suppression does not transfer resource ownership

Merged master !5202 frees the LTE RLC graph error string even on a code path where the caller intentionally suppresses the user-facing error.

**Rule:** deciding not to display/report an error does not imply that an allocated diagnostic object may be leaked. Keep cleanup/ownership independent from presentation policy.

## Terminology: fields are not filters

In closed !5165, Guy Harris corrects the title “Fix a couple of filters”: the changed strings are field names/abbreviations. A display filter is an expression that can contain field names, operators, and values. The MR was closed because unrelated work contaminated the branch, so its code is not precedent; Guy's terminology correction remains highly authoritative.

**Documentation/review rule:** say “display-filter field name/abbreviation” when referring to an `hf_` abbreviation. Reserve “display filter” for the expression/language construct.

## Lower-weight corroboration

!5205 shows a large Qt API migration exposing a missing direct header only under another build environment. !5197 shows a semantic identifier fix needing the same update in parallel dissectors/validation paths. !5203 shows that removing an artificial parse limit can be necessary for correctness but does not remove the obligation to reason about malformed-input work bounds. These reinforce existing notebook guidance and were not promoted as standalone new rules.
