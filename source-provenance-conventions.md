# Wireshark Source Provenance Conventions

This file records durable conventions for incorporating external source code and for deciding whether code belongs in a generic shared utility. Current upstream licensing policy and source remain authoritative.

## Verify provenance and relicensing rights before incorporating third-party code

Code copied from another project is not made safe to redistribute merely by removing its attribution or changing its license header. Before importing an implementation, preserve its provenance and verify that its license is compatible with Wireshark and that any relicensing is actually permitted. If provenance or relicensing rights cannot be established, remove the code and use an independently written implementation if the functionality is still required.

Merged master MR !22415, authored and merged by John Thacker, removes a custom Base32 implementation used by the `fc00` dissector. John notes that the implementation appeared to have been copied verbatim from cjdns without attribution and changed from GPLv3 to GPLv2, with no clear evidence that the contributor had the right to relicense it. The accepted outcome removes the questionable implementation rather than normalizing the copied code; the release-4.6 and release-4.4 backports !22416 and !22418 preserve that resolution.

**Implementation/submission rule:** when contributing externally derived code, identify the source and license and retain required attribution. Do not rewrite copyright/license headers as a substitute for obtaining relicensing permission. When provenance is uncertain, prefer removal or a demonstrably clean-room replacement over carrying ambiguous code into Wireshark.

**Confidence:** Extremely high. Merged master cleanup authored and merged by John Thacker and propagated to two maintained release branches.

## Do not present a protocol-specific algorithm variant as a generic shared utility

Shared utility APIs should have semantics that match the generic name they expose. A protocol-specific encoding variant should either remain local to that protocol or be represented by an API that explicitly models the variant; otherwise callers can reasonably assume standard behavior that the implementation does not provide.

The same merged MR !22415 notes that the removed cjdns routine used a custom Base32 alphabet and therefore was not appropriate as an unqualified generic `base32` facility in `wsutil`. John explicitly observed that a shared API could support multiple Base32 variants if they were modeled deliberately, but a protocol-specific custom alphabet should not silently define what a generic shared helper means.

**Architecture rule:** before moving protocol code into `wsutil` or another shared layer, ask whether the operation really implements the standard/general abstraction suggested by its name. If semantics vary by protocol or alphabet/profile, make that variation explicit in the API or keep the implementation with the protocol-specific consumer.

**Confidence:** Extremely high. This architectural concern is stated directly in the rationale for a merged master cleanup authored and merged by John Thacker.

## Generated source must be reproducible from inputs the project is legally able to retain and use

Checking generated C into the tree does not eliminate the maintenance requirement for its generator inputs. If future asn2wrs, compiler, API, or generator changes require regeneration, Wireshark must be able to possess the authoritative input and legally run the regeneration itself. A workflow that depends on an outside contributor periodically regenerating opaque or unavailable source is not a reproducible upstream maintenance model.

Closed draft MR !22117 proposed generated SAE J2735:2024 dissector code while the underlying ASN.1 specification/source could not be retained in the Wireshark tree under usable redistribution terms. Anders Broman explicitly rejected the proposed workaround of having the contributor regenerate the C externally whenever needed: Wireshark's generated dissectors must be maintainable when asn2wrs or the generated-code API changes, and CI expects generated output to be reproducible rather than hand-maintained. Because the MR was not merged, it is negative-policy evidence rather than accepted implementation evidence, but the maintainer rationale is direct and durable.

**Submission rule:** before proposing generated dissector/source output, verify that the project can retain or otherwise legally access and use the canonical generator inputs under terms compatible with ongoing upstream maintenance. The regeneration procedure must be reproducible by Wireshark developers and CI without depending on a particular contributor or an externally supplied regenerated snapshot.

**Confidence:** High for submission policy. The motivating MR closed unmerged, so it carries less weight than a merged implementation, but the rejection and reproducibility rationale were explicit maintainer feedback about a fundamental generated-source maintenance constraint.