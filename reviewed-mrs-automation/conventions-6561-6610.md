# Durable conventions from Wireshark MRs !6561-!6610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Generated sources: repair the source-of-truth path before touching the derivative

!6564 directly edited generated `packet-skinny.c`. Jörg Mayer immediately pointed out that the file says it must not be modified. Martin Mathieson then found two deeper problems: the generated-file detector expected `Generated automatically` while the file said `Generated Automatically`, and the documented regeneration command/tooling was stale. Because the generator itself failed under Python 3, merged !6587 reverted the generated-output change. Guy Harris reproduced the Python failure on macOS and identified the generator/parser as needing Python 3 work; John Thacker later pointed to !8777.

**Rule:** generated-file recognition is part of the build/checker contract and must match the repository's actual canonical headers. If regeneration is broken, fix the generator/template/tooling first and then regenerate. Do not make a derivative file look correct by hand while the authoritative path remains unable to reproduce it.

## TCP out-of-order reassembly: preserve arrival identity while processing in logical order

Merged master !6567, authored by John Thacker, stops waiting for a later unrelated segment before dissecting PDUs whose missing TCP segment has now arrived. Out-of-order segments are kept in sequence order and fed forward when the contiguous frontier advances. A new reassembly entry point accepts the fragment's original frame number even when the fragment is being incorporated while processing another packet, while the current packet still determines reassembled-in state.

**Rule:** separate three identities in out-of-order stream reassembly: wire sequence position, original arrival/frame identity, and the packet that currently makes reassembly possible. Reordering processing for correctness must not rewrite provenance, and subsequent passes must reproduce first-pass results.

## Qt model traversal must follow the operation's semantic data set

Merged master !6580 and Guy Harris-authored/backport work !6594/!6595 fix interface statistics when some interfaces are hidden. Updating statistics is an operation over every underlying interface, so the loop uses the source model's row count even though view/proxy mappings are still used to reach displayed indices.

**Rule:** use the proxy model when the operation means "the rows the user currently sees"; use the source model when it means "all underlying entities, including filtered/hidden ones." Mapping through a proxy does not imply that the proxy's cardinality is the right iteration domain.

## Use semantic conversion helpers instead of compensating around the wrong API

In merged !6601, Alexis La Goutte first flags a prohibited `sprintf`. John Thacker then identifies the actual issue: `val_to_str_ext_const()` treats its fallback as a constant string, while `val_to_str_ext()` treats it as a format string. The minimal accepted fix changes the helper and needs no temporary formatting buffer.

**Rule:** when an API family has constant-fallback and formatted-fallback variants, choose the variant matching the intended semantics. Do not add manual formatting merely to compensate for calling the wrong helper; doing so increases code and can trigger prohibited-API or buffer-safety issues.

## Display-filter language changes are end-to-end compatibility work

The arithmetic series !6562, !6568, !6575, !6577, !6598, and !6608 changes scanner tokens, grammar and precedence, semantic typing, ftype operations, VM instructions, documentation, release notes, and regression tests. !6598 makes AND bind tighter than OR; !6608 shows that adding symbolic operators also requires revisiting adjacent-token rules so expressions such as `66+1` work without breaking MAC/IP/CIDR literals. User feedback on modulo in !6568 exposed an LHS gap that !6575 then fixed.

**Rule:** treat parser-language changes as compatibility-sensitive, end-to-end features. Update lexical boundaries, precedence/associativity, AST/semantic typing, execution, diagnostics, docs/release notes, and tests together. Include tests for operators adjacent to literals/fields and for realistic user expressions, not only spaced happy paths.

## Submission and reproducer hygiene

In merged !6603, John Thacker advises putting the issue number in the commit message so GitLab closes it automatically and attaching a capture that demonstrates a dissector bug to the issue. In closed !6578, Alexis La Goutte asks that the fix be made on master first; Guy Harris later notes that !6594 is the stable-branch cherry-pick of the master fix from !6580.

**Rule:** make the commit/MR self-linking and reproducible: reference the issue in the commit and attach a minimal capture for packet-decoding defects. Establish fixes on master first, then cherry-pick or backport to maintained release branches unless maintainers explicitly direct otherwise.

## Cross-platform build changes need coverage across supported architecture variants

In merged !6585, a macOS setup change was tested by the author on Intel but not Apple Silicon. Roland Knall explicitly asked for both and then supplied Apple Silicon validation himself.

**Rule:** a platform condition can still have architecture-specific behavior. For build and provisioning changes on a supported platform, cover each materially different supported architecture; when one contributor lacks hardware, reviewer-supplied validation is a legitimate way to close the matrix.
