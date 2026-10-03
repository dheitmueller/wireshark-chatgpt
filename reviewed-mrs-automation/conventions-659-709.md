# Durable conventions from Wireshark MRs !659-!709

This synthesis records reusable lessons from the !659-!709 review batch. Merged MRs are weighted above closed work; Guy Harris-authored/merged changes and direct maintainer corrections receive especially high weight.

## Capture metadata and Wiretap

- **Propagate metadata incrementally when it can appear mid-stream.** A converter must not assume all Interface Description Blocks are available when the input file is opened. Fetch and emit newly discovered IDBs as records are processed so later packets never refer to metadata the output has not received (!669, Guy Harris).
- **Query capabilities, not format names.** If the condition is “does this output format use interface IDs?”, call the capability API rather than special-casing pcapng (!677, Guy Harris).
- **Centralize typed allocation-plus-copy contracts.** When copying a Wiretap block always requires allocating a destination of the corresponding block type, provide one semantic “make copy” helper and make its ownership contract explicit (!667, Guy Harris).

## Parser and dissector dispatch

- **Keep invalid sentinels outside the valid result domain.** A fast parser must not reserve a bit pattern that valid input can produce. !707's Ethernet-address fast path accidentally treated the high bit as an invalid marker and therefore rejected valid octets 0x80-0xff. Use a wider/intermediate representation in which failure is distinguishable from every legal result.
- **Fast paths must preserve the reference grammar.** If multiple separators are accepted, capture the selected separator and require consistency through the complete token instead of accepting mixed forms accidentally (!707, Peter Wu review).
- **Default-enabled heuristics should combine several independent structural invariants.** Cheap length, opcode, count, and range constraints should eliminate unrelated traffic before full dissection. Record the false-positive rationale for default enablement and revisit it when the accepted domain expands (!696).
- **Generic Data fallback is terminal, not a substitute for discovery.** Add raw-data presentation only after the protocol's explicit and heuristic dispatch paths have had their intended opportunity to claim a payload (!675, Peter Wu review).

## Static analysis, fuzzing, and defensive state

- **Do not repair an invalid caller contract inside a low-level callback merely to silence a checker.** If a hash-table callback cannot validly receive NULL, trace the producer that can create the invalid key and fix or enforce the precondition there. Do not map the invalid state onto an ordinary valid hash value such as zero (!704 review discussion; implementation closed/unmerged, so this is review guidance only).
- **Do not imply fuzz-crash causality without evidence.** Reporting the current HEAD is useful provenance, but observing a failure at that revision does not prove the latest commit introduced it. Diagnostics should say “latest” rather than “culprit” unless bisection or an equivalent causal test established that relationship (!685, Gerald Combs).
- **Zero-initialize aggregate state when consumers can observe members not explicitly assigned on every construction path.** Prefer `wmem_new0` where that establishes the intended invariant and avoids indeterminate member state (!694/!695).

## Public scripting APIs and generated documentation

- **Generated docs are part of the WSLua API contract.** When a valid public attribute name falls outside the documentation generator's identifier grammar, fix the generator grammar rather than hand-maintaining one exceptional symbol. After a WSLua API change, verify the generated guide includes the symbol and its mutability correctly (!659, !673; Peter Wu review).

## Reuse already-parsed values

- **If a helper parses a wire value and the caller also needs that semantic value, return it through the helper contract rather than re-reading the bytes.** An optional out-parameter is appropriate when not every caller needs the value (!660, Anders Broman review). This avoids duplicate offset/encoding logic and keeps one parse authoritative.

## Field metadata and widths

- **Registered field type, mask, item length, and access offset must describe the same wire field.** The merged corrections in !690, !682, !681, and !664 independently reinforce this rule. Checker cleanliness is not enough; confirm the actual wire width and location.

## Submission and review workflow

- **Enable maintainer edits when project workflow expects maintainers to rebase or make minor corrections.** A branch that maintainers cannot adjust can block otherwise-ready work (!693, Anders Broman).
- **An amended commit message does not automatically update the GitLab MR description.** Keep the MR description synchronized manually when the revised explanation matters to reviewers (!707, Peter Wu).
- **New dissectors should arrive with representative capture material and without template debris or magic transport constants.** Use available symbolic protocol constants and give reviewers data that exercises the new path (!698, Alexis La Goutte).

## Root cause before broad workaround

Merged !709 removed a shared URL helper after one translation unit failed to compile it, but already-reviewed !719 found that the translation unit simply omitted the header declaring the helper and !721 reverted the broad workaround. Treat the accepted final sequence as the precedent: when a shared abstraction fails in one translation unit, verify includes, declarations, and local API contracts before replacing the abstraction across unrelated callers.

No SMPTE ST 291/VANC packet type was encountered in this batch.
