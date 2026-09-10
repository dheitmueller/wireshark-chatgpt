# Protocol nesting and token-boundary conventions

## Track semantic protocol nesting explicitly

Merged MR !24261 fixes nested TLS sessions and contains substantial review by John Thacker. The initial design keyed TLS session state using the current protocol/layer numbering, but testing showed that `pinfo->curr_proto_layer_num` does not distinguish a genuine nested TLS invocation from another invocation of the same TLS dissector caused by mechanisms such as TCP desegmentation. Treating those as the same concept produced incorrect session selection and test failures.

When state must be separated by true nesting of the same protocol, use explicit per-protocol nesting depth rather than generic dissector invocation/layer order. The accepted !24261 implementation uses `p_set_proto_depth()` so nested TLS/DTLS state follows semantic protocol depth and restores the depth when leaving the dissector. This is complementary to the existing `architecture.md` guidance from !25853: once the correct nesting level is known, pass an explicitly resolved session/context to helpers instead of asking generic lower-level helpers to infer which layer the caller meant.

The same review also exposed an API-contract distinction: callers that only need to inspect existing conversation state should have a lookup operation that can return `NULL`, rather than being forced through a get-or-create API that mutates state. Name and design lookup versus creation APIs according to that behavioral contract.

## Keep length-delimited tokens bounded when calling C-string parsers

Merged MR !24298 fixes lifetime/uninitialized-state problems in the JSON wiretap reader. During review, John Thacker pointed out that a JSON token is length-delimited inside a larger buffer, while `iso8601_to_nstime()` consumes a C string. Passing a pointer into the original JSON buffer without terminating the token can let the parser examine bytes belonging to the rest of the JSON object and potentially accept a false result.

When a parser API expects a NUL-terminated string but the source format supplies a length-delimited token, isolate exactly that token and provide explicit termination before calling the parser. Do not rely on unrelated bytes after the token to stop parsing. This rule applies even when the surrounding source buffer itself is valid and NUL-terminated: the semantic boundary is the token boundary, not the enclosing buffer boundary.

## Provenance

- !24261: merged nested-TLS session fix; John Thacker identified the semantic-depth versus reinvocation distinction and drove the `p_set_proto_depth()` approach.
- !24298: merged JSON wiretap fix; John Thacker explicitly raised the risk of passing the remainder of the JSON buffer to a C-string timestamp parser instead of a token-bounded copy.
