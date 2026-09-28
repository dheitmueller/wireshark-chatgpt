# Durable conventions extracted from MRs 6161–6210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Parser progress is a semantic postcondition

Merged MRs 6163, 6174, and 6175 show two complementary checks. A derived next offset must be validated for minimum structure size and arithmetic wrap before use. After a nested parser or opcode handler returns, a loop that depends on its return value must verify that the parser moved strictly forward. On malformed input, terminate consumption rather than feeding the same offset back into the loop.

Guy Harris's merged MR 6162 sharpens the predicate: no progress means `new_offset <= old_offset`. An earlier defensive fix reversed that comparison and rejected normal nonempty elements. Progress fixes therefore need regression cases for malformed no-progress input and valid positive-progress input.

Merged MRs 6166/6167 and 6168/6169 add two more defenses: cap iterations in nested data whose structure comes from the input, and bound decoded values to a usable arithmetic domain before downstream size calculations. Merely detecting integer wrap does not make enormous but representable sizes practical.

## Packet scope does not eliminate intra-packet stale state

Merged MR 6161 replaces CMS globals that pointed at packet-owned objects with a packet-scoped proto-data structure. That fixes cross-packet lifetime hazards, but the patch also clears individual OID and content-TVBuff slots immediately before each decode that repopulates them. Multiple PDUs or semantic substructures can occur in one packet, and a failed decode must not inherit a value produced by an earlier PDU.

The durable rule is two-level: choose storage lifetime no longer than every object referenced by the state, and separately reset semantic slots at the boundary where each new value is expected. Scope controls lifetime; explicit reinitialization controls freshness.

## Field display metadata belongs to a type-specific semantic domain

Guy Harris's merged MR 6181 makes `proto.c` validation describe and enforce which display-information categories each field type accepts. Accidental switch fallthrough had allowed `BASE_PROTOCOL_INFO` for floating-point fields even though it has no semantic meaning there.

Field registration and checking should express valid type/display combinations directly. Avoid fallthrough between unrelated type families, and make diagnostics tell the contributor which display metadata is permitted.

## Reused owner records must be reset on every path that abandons their payload

Merged MR 6188 and stable MR 6189 fix Find Packet by resetting a `wtap_rec` after an unsuccessful search before the next record replaces it. A loop that reuses an owner object must release or reset its current owned payload on every path that proceeds to another iteration, not only on success or final teardown.

## CI placement should preserve useful pre-merge coverage while controlling cost

Gerald Combs's merged MR 6205 moves slow Ubuntu package production to post-merge, but adds a package test and moves latest-Clang compilation into merge-request pipelines because that compiler was catching defects relevant to macOS. Deferring expensive artifact production is reasonable only when a cheaper validation step still exercises the important packaging/build contract.

Merged MRs 6192 and 6193 add a publishing rule: a release-branch documentation job must not write into an unversioned destination representing master. If publication cannot distinguish branch/version identity, disable it until the destination layout can.

## Public API promotion includes ABI/export bookkeeping

Merged MR 6191 turns `dissect_bluetooth_common()` into public API and updates both the public export declaration and Debian symbols manifest. Removing `static` is not sufficient: exported API changes must be reflected in platform/export declarations and packaging ABI manifests.

## Preserve external-tool path contracts during repository-layout cleanup

Merged MR 6199 relocates Debian packaging inputs under `packaging/debian`, updates repository consumers, and retains a `debian` symlink where Debian tooling requires that conventional name. Centralize the authoritative files while adapting rigid external tools at the boundary rather than duplicating packaging state.

## Submission workflow is corroborated by MRs 6195 and 6196

Closed MR 6195 contains direct Uli Heilmeier review guidance: use the Wireshark component before the colon in the commit subject and source the MR from a dedicated feature branch, not a long-lived personal `master`. The contributor followed that guidance in MR 6196, which merged. This corroborates the notebook's existing submission conventions.

## Do not use UI selection mutation merely as an implicit refresh signal

Merged MR 6202 initially cleared Qt selection to force `selectionChanged()` and refresh packet details when a search result stayed on the same row. A later report showed that the synthetic selection change broke packet-history navigation. Treat model/selection state as user-visible application state, not just a notification mechanism.
