# Conventions extracted from Wireshark !7811-!7860

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

## Normalize TVBuff coordinates before applying open-ended lengths

Merged !7849, authored by John Thacker, fixes `tvb_new_subset_length()` for a nonzero offset and reported length `-1`. The accepted code resolves the subset offset first, bounds-checks it, and then computes the remaining reported length from that resolved start.

Guy Harris adds that APIs using signed offsets are awkward when a protocol supplies an offset in an unsigned field.

**Rule:** resolve caller-facing coordinate conventions before deriving "remaining" bounds, and preserve the signedness domain of packet-derived offsets.

## Propagate helper failures and preserve the local status contract

Merged !7832 adds handling for failure of a TLS helper initialization step. Pascal Quantin directed the implementation to return failure to the parent, free allocations on error paths, and follow the surrounding 0-success / -1-failure convention.

**Rule:** propagate lower-level failure to the layer that owns control flow or reporting, unwind resources before returning, and match the component's established status convention.

## Preserve persisted preference names across semantic renames

Merged !7812 renames TCP's experimental-option preference while explicitly mapping the old persisted key to the new preference during preference loading.

**Rule:** a persisted preference rename is a compatibility change. Add an old-name-to-new-name migration or alias unless existing configuration is intentionally being discarded.

## Keep focused MRs focused when review exposes a broader old defect

In merged !7819, Pascal Quantin notices a length-validation issue that also applies to many existing IEs. He and the contributor agree that the broader cleanup should be separate from the focused MR adding new IEs.

**Rule:** when review uncovers a broad pre-existing issue that is not required for the proposed change to be correct, record it and use a separate follow-up rather than expanding the MR into an unrelated subsystem cleanup.

## Backports must include prerequisite chains

During merged release-4.0 !7824, Pascal Quantin identifies prerequisite changes needed for the requested backport, and the contributor notes the release branch also needs the required dependency capability.

**Rule:** evaluate a stable-branch backport as a dependency-closed change. Verify prerequisite code and dependency versions are already present or are backported in a reviewable order.

## Supply representative captures and keep backportable history clean

Merged !7842 adds BGP-MUP support. Alexis La Goutte requests a representative pcap; the contributor supplies focused coverage across the new message forms. Alexis also asks for the changes to be squashed manually because that is easier to backport. Merged !7830 independently includes a requested focused 802.11 reproducer and before/after validation.

**Rule:** protocol changes should include focused capture evidence for the new or fixed paths. For changes likely to be backported, keep the commit history clean and directly cherry-pickable.

## Retained address state must own storage with a matching lifetime

Merged release-4.0 !7827 fixes retained RDPUDP address state by copying it into file-scope wmem storage. This corroborates the stronger address-lifetime rules already in the notebook.

## Prefer event-driven common abstractions over platform-specific polling

Merged !7852 replaces separate sync-pipe handling paths with a GIOChannel watch, removing Windows periodic pipe polling and UNIX-specific common-path handling.

**Rule:** when a cross-platform event abstraction provides the needed readiness semantics, prefer it over parallel OS-specific polling loops.

## User-facing file-format names should explain opaque acronyms

Merged !7851, authored by Guy Harris, replaces an opaque BLF label with a description that spells out Vector Informatik Binary Logging Format.

**Rule:** user-facing format descriptions should be understandable without prior knowledge of an acronym.
