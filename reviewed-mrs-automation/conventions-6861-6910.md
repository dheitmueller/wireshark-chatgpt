# Durable Wireshark conventions from !6861-!6910

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

## Static-analysis fixes must preserve semantic side effects

Merged !6909 demonstrates that "unused value" and "dead statement" are different propositions. Guy Harris caught an analyzer cleanup that removed parser-offset increments along with unused assignments. When addressing warning output, audit cursor changes, helper side effects, validation behavior, and control-flow consequences before deleting an expression.

## Similar conversation flags require a full lifecycle audit before consolidation

Closed !6901 is strong negative evidence. Gerald Combs initially believed `NO_PORT2_FORCE` and `NO_PORT2` were equivalent because construction selected the same wildcard table, then discovered they behaved differently when a conversation's second port was later specialized and closed the MR. Compare creation, lookup, mutation, and specialization semantics, not just the initial table choice.

## Nested instances of one protocol need instance-qualified state

Merged !6894 allows tunneled EAP/TLS to contain another EAP instance by distinguishing per-packet and conversation state according to the current protocol-layer instance. Protocol ID alone is not a sufficient state identity when a dissector can recur within one frame. Duplicate detection, reassembly state, and child-dissector state must be keyed to the logical instance.

## Use the registered protocol as the protocol tree root

Merged !6893, authored by Guy Harris, replaces an anonymous USB packet subtree with a tree item created from `proto_usbll` and then attaches the protocol subtree beneath it. A dissector's root item should normally carry the registered protocol identity.

## Table-driven refactors should encode semantics in the table

In merged !6902, Jaap Keuter rejected magic type values in a SIP parameter table and asked for explicit table metadata describing whether each parameter is string or integer. He also asked that a local lookup index be scoped where it is actually used. A table-driven refactor is maintainable when the table expresses the dispatch semantics rather than pushing hidden conventions into control flow.

## Parser progress must follow each nested object's declared extent

Merged !6892 fixes TEAP TLV parsing by advancing by each TLV's actual declared length and keeping subsequent TLVs out of the preceding TLV's subtree. The supplied capture showed the old parser stopped/attached later TLVs incorrectly. For repeated length-delimited structures, each iteration must consume exactly its own extent and tree ranges must close on the same boundary.

## Private helper framing heuristics should use a strong signature

Merged !6889 adds a development-oriented NAS-5GS UDP framing heuristic that first checks enough captured bytes for the complete signature plus payload, requires the exact signature, and is disabled by default. Nonstandard helper transports should not claim arbitrary UDP merely because content can be decoded after the fact.

## New portable capture link types should be externally assigned first

Merged !6884 was held while FiRa UCI's portable link type was still awaiting tcpdump/libpcap registry assignment. After LINKTYPE_FIRA_UCI was assigned as 299, the MR was updated and merged. Wireshark's internal WTAP encapsulation number is a separate namespace. A new encapsulation should include the portable mapping, internal mapping, representative capture, dissector registration, and user-visible release/documentation updates.

## Capture-learned state that changes over time needs frame-relative history

Merged !6872 changes PDCP-LTE from one current configuration per UE to a bounded sequence of updates tagged with their setup frame, then selects the newest update applicable to the frame being dissected. This preserves correct behavior when users move backward and forward through a capture or packets are redisected after later state has been learned. Last-value global state is insufficient for time-varying protocol configuration.

## Typed-item checks are structural feedback, not semantic proof

Merged !6871 expanded the then-current CI typed-item checker invocation with `--consecutive --label --mask` while leaving findings warning-only. The historical flags should not replace today's stronger command; current repository tooling is authoritative. Review also clarified that changing an MR title does not change the commit message checked by CI: fix commit metadata by amending the commit itself.

## Supported build correctness outranks silencing one analyzer

Merged !6870 reverts a qcustomsplot change that initialized Qt iterators with integer zero to quiet a Clang warning because Qt 6.3 no longer accepted that conversion. Do not keep a static-analysis workaround that violates the actual API/type contract of a supported dependency version. Prefer a narrow diagnostic suppression or another valid representation.

## Temporary packet context should borrow address storage when it does not own it

Merged !6864 fixes a leak in a stack-local `packet_info` copy by changing deep `copy_address()` calls to `copy_address_shallow()` for addresses whose storage already outlives the temporary context. Copy mode follows destination ownership and lifetime, not simply the fact that the field type is `address`.

## Generated semantic-name fields can improve filtering without replacing wire fields

Merged !6874 adds generated, hidden SOME/IP string fields for resolved service/method/client names while retaining the numeric on-wire identifiers. When resolution metadata is useful for filters but is not itself present on the wire, expose it as a generated semantic field rather than pretending it occupies packet bytes or replacing the raw field.

## Fuzz/test diagnostics should report evidence rather than guess a culprit

Merged !6865 changes fuzz diagnostics from showing only the latest commit to showing all commits from the preceding 48 hours. The latest commit is not necessarily the source of a delayed or environment-dependent failure. Diagnostic output should provide the relevant candidate window and let investigation establish causality.

## Weighting notes

Merged master MRs with direct maintainer input are the primary evidence. !6893 carries especially high authority because it is authored by Guy Harris. !6909 carries high authority because Guy identified the semantic regression during review. Closed !6901 is retained only as negative API evidence; the rejected simplification itself is not an implementation exemplar. Closed !6906, !6904, and !6903 are not promoted as accepted design.
