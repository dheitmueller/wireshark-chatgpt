# Wireshark Parser Recursion Conventions

This file records durable recursion and dissection-depth conventions extracted from upstream Wireshark merge-request review. Current upstream source remains authoritative.

## Treat unconstrained dynamic/open types as potentially recursive

When generated protocol structure allows a runtime-selected type whose possible targets cannot be bounded statically, absence of a known dependency edge is not proof that recursion is impossible. Conservatively route such paths through normal dissection-depth protection.

Merged release-branch MRs !24315 and !24316, authored and merged by John Thacker, change `asn2wrs` handling of unconstrained ASN.1 open types. Because an open type can carry values whose type is not known at specification time, generated dependency analysis cannot prove that it is outside a cycle. The accepted implementation therefore treats an otherwise-unresolved open type as cyclic with itself so generated dissection receives depth checks.

**Implementation rule:** if a dynamic dispatch/type mechanism can reach an unknown set of parser targets, assume recursive reachability unless the schema proves otherwise. Favor a bounded false positive in recursion protection over an unbounded parser path.

**Confidence:** Very high. The behavior is preserved in two merged stable branches by John Thacker and is justified from the ASN.1 open-type semantics in the MR rationale.

## Mutate dissection depth through checked helper APIs, including multi-level adjustments

Dissection depth is an invariant-bearing safety counter, not ordinary scratch arithmetic. Code that conceptually crosses several generated dependency levels should use a helper that applies the full adjustment with overflow/underflow checks and should undo the same amount through the matching helper.

Merged release-branch MRs !24311 and !24314, authored and merged by John Thacker, introduce/use `increment_dissection_depth_by_n()` and `decrement_dissection_depth_by_n()` rather than directly modifying `pinfo->dissection_depth` around calls. Besides centralizing overflow/underflow checking, the change corrects generated paths that had previously incremented and decremented by one level too many.

**Implementation rule:** do not directly add to or subtract from `pinfo->dissection_depth`. Express the semantic number of recursion levels through the checked depth helpers, and keep increments/decrements exactly paired across every return path.

**Confidence:** Very high. Merged safety API and generated-code corrections authored and merged by John Thacker on two maintained branches.

## Suppress recursion warnings only after proving monotonic parser progress

Recursive parsing is not safe merely because normal captures happen to terminate. If the recursive call is driven by packet-controlled offsets or lengths, malformed input must be unable to make the parser recurse without consuming input, move backward, or wrap an offset back into an earlier region.

Merged MR !22414 underwent extended review before merge. In response to clang-tidy's `misc-no-recursion` warning, John Thacker explained that a local suppression can be reasonable only after showing that every recursive step advances the packet offset and that arithmetic cannot overflow, wrap, or otherwise leave the parser at the same position under fuzzed or malformed input. The warning is useful precisely because it forces that termination argument to be made rather than assuming recursion is harmless.

**Implementation rule:** before suppressing a recursion diagnostic in a packet parser, establish a monotonic progress invariant for all input-controlled paths: each recursive descent must consume a positive bounded amount or move to a strictly later position, and the coordinate arithmetic must not overflow/wrap. If that proof is awkward, add an explicit depth/work bound or restructure the parser iteratively rather than silencing the warning.

**Confidence:** Very high. Direct John Thacker review on a substantial MR that ultimately merged after prolonged review and revision.

## Put an explicit depth limit on externally controlled nesting

Even when each individual nesting step makes progress, an input grammar whose nesting depth is controlled by a capture, dictionary, schema, or other external input needs an explicit maximum depth. Natural termination of valid input is not a resource bound for malformed or adversarial input.

Merged master MR !16197 adds a TLV nesting stack to the RADIUS dictionary parser and rejects nesting at `MAX_INCLUDE_DEPTH`, emitting a diagnostic and terminating the parse instead of allowing an unbounded nesting chain. The same existing bound was already used for include nesting, making the parser's finite nesting budget explicit and auditable.

**Implementation rule:** for recursively or stack-parsed external nesting, define a finite maximum depth and check the bound before pushing/descending. The failure path should stop parsing that input cleanly and report the offending nesting where practical. Do not rely on C stack exhaustion, allocator failure, or well-formed-input expectations as the effective limit.

**Review rule:** distinguish progress safety from depth safety. A parser can advance monotonically on every level and still consume excessive stack, heap, or CPU if the number of levels is unbounded.

**Confidence:** High. Accepted merged parser hardening in the RADIUS dictionary loader, consistent with Wireshark's broader bounded-dissection model.

## Recursion protection is an explicit project policy, not merely a static-analysis cleanup

Wireshark's developer guidance treats recursive dissection as a safety concern in its own right. A dissector that recurses must be able to show why its recursion is bounded; when the protocol does not supply a natural finite bound, it should use Wireshark's dissection-depth facilities rather than relying on the C call stack.

Merged master MR !15189 was authored by Gerald Combs and merged by Anders Broman specifically to add this requirement to `doc/README.developer`. The documentation points developers at `increment_dissection_depth()` / `decrement_dissection_depth()` and notes that CI's clang-tidy recursion check may require a suppression only after the recursion has been made safe. This gives the implementation patterns recorded elsewhere in this file an explicit project-policy anchor.

**Submission rule:** a new or modified recursive dissector should make its termination/depth argument visible in the code and review. If recursion is inherently bounded by protocol structure, document or preserve that invariant; otherwise use the checked dissection-depth helpers. Do not treat silencing `misc-no-recursion` as the objective—the objective is bounded work and bounded stack use on malformed captures.

**Confidence:** Extremely high. This is merged project developer documentation authored by Gerald Combs and accepted by Anders Broman, so it represents unusually direct evidence of intended Wireshark contributor policy.
