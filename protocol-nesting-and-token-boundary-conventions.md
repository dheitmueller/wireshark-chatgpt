# Protocol nesting and token-boundary conventions

## Track semantic protocol nesting explicitly

Merged MR !24261 fixes nested TLS sessions and contains substantial review by John Thacker. The initial design keyed TLS session state using the current protocol/layer numbering, but testing showed that `pinfo->curr_proto_layer_num` does not distinguish a genuine nested TLS invocation from another invocation of the same TLS dissector caused by mechanisms such as TCP desegmentation. Treating those as the same concept produced incorrect session selection and test failures.

When state must be separated by true nesting of the same protocol, use explicit per-protocol nesting depth rather than generic dissector invocation/layer order. The accepted !24261 implementation uses `p_set_proto_depth()` so nested TLS/DTLS state follows semantic protocol depth and restores the depth when leaving the dissector. This is complementary to the existing `architecture.md` guidance from !25853: once the correct nesting level is known, pass an explicitly resolved session/context to helpers instead of asking generic lower-level helpers to infer which layer the caller meant.

The same review also exposed an API-contract distinction: callers that only need to inspect existing conversation state should have a lookup operation that can return `NULL`, rather than being forced through a get-or-create API that mutates state. Name and design lookup versus creation APIs according to that behavioral contract.

## Keep length-delimited tokens bounded when calling C-string parsers

Merged MR !24298 fixes lifetime/uninitialized-state problems in the JSON wiretap reader. During review, John Thacker pointed out that a JSON token is length-delimited inside a larger buffer, while `iso8601_to_nstime()` consumes a C string. Passing a pointer into the original JSON buffer without terminating the token can let the parser examine bytes belonging to the rest of the JSON object and potentially accept a false result.

When a parser API expects a NUL-terminated string but the source format supplies a length-delimited token, isolate exactly that token and provide explicit termination before calling the parser. Do not rely on unrelated bytes after the token to stop parsing. This rule applies even when the surrounding source buffer itself is valid and NUL-terminated: the semantic boundary is the token boundary, not the enclosing buffer boundary.

## Bound delimiter searches to the grammar element being parsed

A delimiter that is meaningful inside one token must be searched for only inside that token. Searching the remainder of a tvbuff can find the same delimiter in a later parameter and incorrectly reinterpret the current token as if it contained a delimiter that is actually outside its grammar boundary.

Merged master MR !13557, authored and merged by John Thacker, fixes SDP `fmtp` parsing this way. The dissector previously called `tvb_find_guint8()` with a length of `-1` while looking for `=` in the current media-format parameter. If the current token had no `=` but a following parameter did, the later delimiter could be mistaken for one belonging to the current token. The accepted change bounds the search to `tokenlen`; release-4.2 backport !13558 preserves the same fix.

**Implementation rule:** once a parser has identified the length or end of the current token/container, pass that bound to delimiter/search helpers. An enclosing tvbuff's availability is not the same thing as the current grammar element's extent.

**Review rule:** test adjacent tokens where the first omits an optional delimiter and a later token contains it. This catches parsers that accidentally let a search escape the element currently being decoded.

**Confidence:** Very high. Merged master fix authored and merged by John Thacker, with an accepted stable-branch backport and a concrete cross-token misparse described in the MR.

## Split expressions only at real tokens in the intended nesting level

Expression text cannot safely be decomposed by matching delimiter substrings without also respecting lexical token boundaries and grouping depth. An operator spelling may occur inside an identifier or inside a parenthesized subexpression where it does not delimit the outer construct being extracted.

Merged master MR !14361, authored and merged by John Thacker, fixes custom-column expression splitting in two ways: textual `or` is recognized only as a standalone operator rather than as an arbitrary substring, and `or`/`||` operators inside parentheses are not treated as separators for the outer multi-field custom column. The MR also relies on the display-filter grammar's OR precedence to explain why top-level OR is the correct separator for the supported multi-field form.

**Implementation rule:** when extracting top-level components from a language expression, tokenize or otherwise enforce the language's real operator boundaries and track grouping depth. Split only on operators that belong to the intended syntactic level. A regex is acceptable only if it actually models those lexical and nesting constraints; plain substring matching is not.

**Review rule:** test operator spellings embedded in identifiers, nested parentheses, and combinations of nested and top-level operators. Check the grammar's precedence rather than assuming that a visually convenient separator has the semantics the caller needs.

**Confidence:** Very high. Merged master parser/column fix authored and merged by John Thacker, with both token-boundary and parenthesis-depth requirements stated explicitly in the MR.

## Provenance

- !24261: merged nested-TLS session fix; John Thacker identified the semantic-depth versus reinvocation distinction and drove the `p_set_proto_depth()` approach.
- !24298: merged JSON wiretap fix; John Thacker explicitly raised the risk of passing the remainder of the JSON buffer to a C-string timestamp parser instead of a token-bounded copy.
- !13557 and !13558: merged master SDP fix and accepted stable backport; delimiter search is constrained to the current `fmtp` token instead of the rest of the tvbuff.
- !14361: merged custom-column expression fix authored and merged by John Thacker; top-level splitting was made token-aware and parenthesis-aware.
