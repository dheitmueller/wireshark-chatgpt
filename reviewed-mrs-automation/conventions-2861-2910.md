# Durable conventions — !2861–!2910

## Gate only the functionality that actually depends on an optional library

Merged !2894 briefly introduced a no-op RTP Player entry point for builds without libpcap. Guy Harris tested that supported configuration, reproduced a crash in the otherwise usable RTP Player path, and authored merged !2905 to remove the guard.

**Rule:** dependency gates should follow the code that actually needs the dependency. Test supported feature-disabled builds at runtime on representative UI and CLI paths; compilation alone is not sufficient.

## Presence flags are assertions about populated metadata

Merged master !2865, authored by Guy Harris, removes `WTAP_HAS_PACK_FLAGS` because the exported-PDU record never set the corresponding packet-flags value. Maintained-branch copies !2866 and !2867 carry the same correction.

**Rule:** set a `WTAP_HAS_*` bit only when the associated metadata has actually been initialized to a meaningful value.

## Express deliberate ignored returns explicitly when the return is not an error channel

In merged !2879, Guy Harris casts `g_strlcpy()` and `g_strlcat()` results to `void` where truncation is already accepted and the returned source length is not needed.

**Rule:** establish the API's real return semantics before reacting to ignored-result diagnostics. If a result is intentionally irrelevant, make that intent explicit rather than inventing fake error handling.

## Code to Wireshark's supported compiler subset

Merged !2886 failed Windows compilation because an empty aggregate initializer was accepted by some compilers but not the supported MSVC configuration. Alexis La Goutte surfaced the failure and Pascal Quantin pointed to the project's documented supported C features and the portable `{ 0 }` form.

**Rule:** portability is governed by Wireshark's declared compiler/language baseline and CI matrix, not by what one preferred compiler happens to accept.

## Let TCP own stream desegmentation before nested parsing

Merged !2861 repairs SMB-Direct over iWARP by routing MPA-over-TCP through `tcp_dissect_pdus()` with a dedicated PDU-length callback.

**Rule:** for framed protocols over TCP, determine complete-PDU length at the transport framing layer and let TCP reassemble missing bytes before invoking deeper decoders.

## Use tree helpers that return decoded values

In merged !2874, Pascal Quantin recommends `proto_tree_add_item_ret_uint()` for a length field so the tree item and caller's numeric value come from one decode operation.

**Rule:** when a typed `proto_tree_add_item_ret_*` helper fits, prefer it to a separate `tvb_get_*` plus `proto_tree_add_item()` sequence.

## Keep submissions focused and amend the actual review unit

Closed !2862 and !2863 are lower-weight than merged implementation evidence, but their maintainer guidance is clear: avoid unrelated whitespace/files, do not duplicate an existing dissector wholesale when common code can be shared, amend the commit to address commit-message review, build before resubmitting, and squash work-in-progress history into a focused unit.
