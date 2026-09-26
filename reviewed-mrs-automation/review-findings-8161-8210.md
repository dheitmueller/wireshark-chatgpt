# Review findings: Wireshark MRs !8161-!8210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs in the exact run ledger are merged. Merged master changes and direct maintainer review were weighted most heavily; stable-branch backports and automatic updates are corroboration.

## Highest-value findings

- **!8204 (John Thacker): raw bytes and semantic text are different interfaces.** HTTP keeps raw header-value bytes for subdissector consumers while storing a separately decoded ASCII-safe value in the protocol tree. This is a strong early example of not forcing opaque wire bytes and text through one representation.
- **!8199 (John Thacker): use the common wire-encoding decoder.** PFCP APN/FQDN handling moves from in-place byte rewriting to `ENC_APN_STR`, avoiding invalid UTF-8 and duplicated decoding logic.
- **!8210 (John Thacker): protocol-specific encoding rules beat ambient mode flags.** The deprecated SMB search commands are explicitly OEM-only and must not inherit generic Unicode handling.
- **!8171, !8196, !8197, !8209 (Gerald Combs): explicit Qt connections.** Menu actions migrate away from name-based auto-connections toward typed explicit connections. !8171 deliberately uses queued delivery for actions that can interact with context-menu teardown or nested event processing.
- **!8165 (John Thacker): reject before claiming.** A dissector associated with a non-IANA/default port performs guarded content recognition first; after a positive TCP match it records conversation state so desegmentation can continue without weakening the negative path.
- **!8177:** TRANSUM's calculated timing analysis claims zero packet bytes rather than highlighting the whole frame, reinforcing that source ranges describe actual byte ownership.
- **!8190 + !8178:** one-character values are represented as `FT_CHAR`, and WSLua is kept consistent with native proto-tree field semantics.
- **!8203 + !8202:** new-dissector review included standard source structure, field type and filter identity, reserved-byte visibility, static-analysis cleanup, release notes, conservative registration, CI availability, and representative captures.
- **!8163 (John Thacker): structured lookup keys matter.** GTP session tracking replaces address stringification plus linked-list scans with a native TEID/address map and reports a greater-than-10x speedup on representative captures.
- **!8184 (John Thacker): serialization legality is distinct from Unicode validity.** XML 1.0 allows TAB/LF/CR among ASCII controls but forbids the others even as character references.
- **!8208 + !8162:** exported public symbols require matching Debian symbol-manifest metadata.

## Weighting notes

!8198 is useful mainly as the precursor that !8204 refines. !8200 is transitional UTF-8 assertion hardening; the later UTF-8 contract work already recorded in the notebook is stronger authority. Release-branch backports and automatic-update MRs were scanned for purpose, discussion, and final diff but did not create independent architectural rules.
