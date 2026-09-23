# Automated Wireshark MR review: !12563 through !12612

## Corpus snapshot

Reviewed against `dheitmueller/wireshark-corpus-mrs` commit `ddcaa22b51c68f594e425a23388c3a2086813054`.

Before selecting this batch, the available review tracking in `reviewed-mrs-automation/`, `reviewed-mrs.md`, and the supplemental notebook tracking was consulted to reconstruct the already-reviewed set. Selection was performed by subtracting that exact set from the MRs present in the corpus and choosing the 50 highest-numbered remaining MRs. The historical reviewed batch !17571-!17620 remains preserved and counted. The earlier lookup of !12612 was only a frontier probe and was not counted as a review before this run.

## Exact MRs reviewed

Exactly these 50 merge requests were reviewed in this run:

!12612, !12611, !12610, !12609, !12608, !12607, !12606, !12605, !12604, !12603, !12602, !12601, !12600, !12599, !12598, !12597, !12596, !12595, !12594, !12593, !12592, !12591, !12590, !12589, !12588, !12587, !12586, !12585, !12584, !12583, !12582, !12581, !12580, !12579, !12578, !12577, !12576, !12575, !12574, !12573, !12572, !12571, !12570, !12569, !12568, !12567, !12566, !12565, !12564, !12563.

All 50 are merged in the corpus snapshot. Merged changes therefore receive full evidentiary weight; no abandoned or superseded MR in this batch needed to be down-weighted.

## Durable findings promoted or reinforced

### Runtime capability can differ from nominal dependency capability

Merged master !12571, authored by John Thacker, handles the case where libgcrypt is present at a sufficient version but runtime FIPS policy disables BLAKE2s and ChaCha20, making WireGuard decryption unavailable. The accepted behavior records the effective capability, avoids entering key/decryption state paths when unsupported, and exposes an expert diagnostic instead of asserting or emitting a noisy unconditional startup warning. Maintained-branch backports !12605 and !12606 reinforce the rule. Related maintained-branch changes !12563 and !12564 also demonstrate that deliberately changing libgcrypt's global FIPS mode has a strict pre-initialization ordering contract. This was added to `platform-capability-detection-conventions.md`.

### Shared CLI options should be owned by their semantic capability

Merged master !12569, authored and merged by John Thacker, fixes documented-but-unimplemented TShark long options and factors common option definitions. Guy Harris gave especially authoritative architectural feedback that `-r`/`--read-file` should not be grouped as a dissection option merely because TShark commonly reads and dissects captures; reading a file is a distinct input capability. John reworked the organization accordingly. Gerald Combs also requested test coverage, and the accepted MR adds a focused long-option test. This was added to `cli-option-semantics-conventions.md`.

### Generic frame code should route extension identifiers rather than own vendor semantics

Merged master !12585 replaces hard-coded pcapng custom-block handling in the common frame dissector with a dissector table and moves format-specific handling into the owning dissectors. This independently reinforces the later, stronger pcapng extension-architecture evidence already in the notebook and was added to `pcapng-extension-architecture-conventions.md`.

### Masked-field value strings use post-mask logical values

Merged !12607, by Martin Mathieson, fixes typed-item checker findings in which `value_string` keys used wire-positioned bit values even though the associated fields had masks. The corrections use the logical post-mask/post-shift values. This is independent earlier evidence for the already-recorded field-value rule and was added to `field-value-semantics-conventions.md`.

## Strong corroborating findings retained without duplicate rules

!12610 and !12604 fix an HTTP/3 ASan stack-use-after-return where stack-backed QUIC stream information escaped into longer-lived state; the supplied sanitizer trace and reproducer strongly corroborate existing ownership/lifetime and sanitizer-testing guidance. !12600, with substantive Guy Harris review, reinforces that frames lacking timestamps must not become predecessor/reference frames for later timestamp-delta calculations. !12597 reinforces first-pass-only conversation-state mutation and packet-local persistence so redissection remains deterministic. !12582 likewise avoids repeated file-scope allocation by guarding HTTP conversation-state mutation with the visited flag. !12574 includes maintainer review requesting a sample capture before accepting transport-decoding expansion, reinforcing existing test-vector practice.

Other merged changes in the batch were protocol-local fixes, release backports, documentation/build cleanup, UI changes, or narrow refactors that did not justify a new durable notebook convention.

## Frontier

!12562 (`gcrypt: Disable FIPS mode if possible`) exists in the corpus snapshot and is merged. It was inspected only to prove that the corpus continues below this batch and is **not** counted as reviewed here. Absent newly scraped higher-numbered unreviewed MRs, !12562 is the next descending candidate.
