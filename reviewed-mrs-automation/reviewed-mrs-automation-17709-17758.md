# Wireshark MR review automation: !17709-!17758

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: newest toward older. Existing tracking in `reviewed-mrs.md` and `reviewed-mrs-automation/` was consulted first. The previously reviewed !17571-!17620 batch remains part of the reviewed set. This run reviewed no more than 50 MRs.

## Exact reviewed set

!17758, !17757, !17756, !17755, !17754, !17753, !17752, !17751, !17750, !17749, !17748, !17747, !17746, !17745, !17744, !17743, !17742, !17741, !17740, !17739, !17738, !17737, !17736, !17735, !17734, !17733, !17732, !17731, !17730, !17729, !17728, !17727, !17726, !17725, !17724, !17723, !17722, !17721, !17720, !17719, !17718, !17717, !17716, !17715, !17714, !17713, !17712, !17711, !17710, !17709

Count: **50**.

## Review notes and durable evidence

- **!17758 — merged / scanned.** Qt minimum-version change from 5.12 to 5.15. Straightforward build-policy update; no substantive human review beyond approval and no new durable convention.
- **!17750 — merged / deep.** TCP bogus-header-length expert information was incorrectly guarded by `if (tree)`. The accepted change removes the tree-presence condition, allowing the protocol item/expert diagnostic path to execute independently of whether a visible protocol tree was requested. This strongly corroborates existing notebook guidance that tree presence is not a proxy for whether non-tree semantic side effects, diagnostics, or column behavior are required.
- **!17736 / !17734 — merged master/backport pair / implementation-focused.** IEEE 802.11 Block Ack parsing for 64/128-byte bitmaps must include the outer 64-bit chunk offset both when fetching bitmap words and when associating missing-frame fields with packet bytes/frame numbers. !17734 is the release-4.4 cherry-pick of the master fix represented by !17736; weight the master change as the primary exemplar and the backport as corroboration.
- **!17715 — merged / deep / high-authority discussion.** Fixes TKIP broadcast-frame decryption for WPA2 mixed-mode networks by using the negotiated group cipher suite rather than treating the EAPOL key version alone as sufficient. Guy Harris explicitly asked that the non-obvious protocol reasoning be tied to the specification. The resulting discussion identified IEEE 802.11 RSNA establishment/cipher-suite selection and distinguished the legacy WPA1 key-version inference from WPA2 cipher-suite semantics. The MR also adds a regression test. Durable lesson: when protocol logic is non-obvious and depends on a standards distinction, leave enough specification/reference context in the code or change rationale that a future maintainer can verify why the branch is correct; regression tests should cover the interoperability case that exposed the distinction.

## Weighting

Merged MRs were treated as stronger implementation evidence than closed, abandoned, duplicate, or superseded work. Backports duplicating a master fix were counted as reviewed but not treated as independent architectural evidence. Maintainer discussion was weighted by authority and specificity; Guy Harris's protocol-semantics request in !17715 was treated as especially strong review evidence.

## Notebook promotion decision

No separate convention file was changed in this run. !17750 directly corroborates already-recorded NULL-tree/semantic-side-effect guidance, and the specification-reference lesson from !17715 is recorded here for later consolidation if additional independent review evidence appears. Avoid creating a new top-level convention from a single protocol-specific example when the existing notebook can retain the evidence in the run ledger.

## Continuation

For the next run, rebuild the already-reviewed set from all ledgers rather than assuming contiguous ranges. Subject to holes in older tracking, continue with the fifty highest-numbered corpus MRs not already present in that set. In the straightforward contiguous case after this ledger, the next candidate begins at !17708. Preserve and count !17571-!17620.