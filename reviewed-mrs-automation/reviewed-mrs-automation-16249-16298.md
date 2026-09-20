# Wireshark MR review automation ledger: !16249-!16298

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Notebook starting commit: `9de251eff1e337b90fc68139a469608dc9681049`

Selection method: enumerated the complete recursive Git tree for the corpus commit, reconstructed the already-reviewed set from `reviewed-mrs.md` and all available per-run files in `reviewed-mrs-automation/`, preserved and counted the historical !17571-!17620 batch, subtracted that exact reviewed set from the corpus MR set, sorted the remainder numerically descending, and selected the first 50. No numeric range was assumed reviewed merely because another MR in that range appeared in a ledger.

Exactly reviewed in this run (50 MRs):

- !16298
- !16297
- !16296
- !16295
- !16294
- !16293
- !16292
- !16291
- !16290
- !16289
- !16288
- !16287
- !16286
- !16285
- !16284
- !16283
- !16282
- !16281
- !16280
- !16279
- !16278
- !16277
- !16276
- !16275
- !16274
- !16273
- !16272
- !16271
- !16270
- !16269
- !16268
- !16267
- !16266
- !16265
- !16264
- !16263
- !16262
- !16261
- !16260
- !16259
- !16258
- !16257
- !16256
- !16255
- !16254
- !16253
- !16252
- !16251
- !16250
- !16249

## Durable findings

- **!16271 — deep, merged master, extremely high authority.** Authored and merged by Guy Harris. `capinfos` must distinguish packet-derived extrema from capture/session boundaries and from ordinal record position: packet timestamps can establish an earliest/latest packet, but not necessarily capture start/end, and capture order need not be chronological. Promoted to `capture-start-time-conventions.md`.
- **!16252 — deep, merged master.** `register_tap()` belongs in the protocol registration routine, not handoff, because peer dissectors may need the tap to exist from their own handoff routines. Promoted to `initialization-lifecycle-conventions.md` as an explicit registration-order rule.
- **!16250 — deep, merged master.** Profile switching reloads a lower-precedence preference layer and must then reapply still-active command-line overrides. The accompanying documentation also makes the deliberate interactive-preference exception explicit. Promoted to `initialization-lifecycle-conventions.md` as a configuration-precedence/lifetime rule.
- **!16268 plus !16265 — deep, merged master.** Martin Mathieson and Stig Bjørlykke highlighted the review risk in very large scripted type-conversion MRs: manual semantic fixups must be distinguishable from mechanical replacements, and successful compilation is not proof that those fixups are correct. !16265 explicitly identifies its exceptional manual change. Promoted to `checker-tooling-conventions.md` as a bulk-refactor reviewability/submission rule. Closed !16278 is used only as secondary negative evidence that an independently meaningful compiler/semantic fix should not be hidden inside an indiscriminate conversion campaign.
- **!16285 — deep, merged master, authored by John Thacker.** Generated protocol types must respect the width actually consumed from the wire, including representation/alignment padding where a protocol encoding such as NDR64 carries more octets than the useful scalar value. This corroborates existing wire-layout and generated-code guidance rather than creating a duplicate rule.
- **!16283 — deep, merged master, authored by John Thacker.** Protobuf Timestamp formatting now follows the protocol's permitted fractional-second widths and refuses nanosecond values outside the valid range instead of formatting invalid state. This reinforces semantic validation before presentation/serialization.
- **!16269 — merged master with substantive review.** Exported-PDU variable-length values require 32-bit alignment. Review explicitly examined whether padding belongs in the common API or the callback/caller contract; the accepted implementation makes the serialized padded size explicit. Treated as corroboration for wire-layout/alignment guidance.
- **!16255 — deep, merged master, authored by John Thacker.** When the BitTorrent PDU-length callback has a complete length prefix but not the message-type byte, returning 0 requests more stream data instead of misclassifying continuation bytes. This strongly corroborates the existing TCP desegmentation callback contract.
- **!16261 — merged, but later negative evidence.** The accepted TCP Bytes-in-Flight change improved ordinary cases but a later John Thacker note records a regression at 32-bit sequence-number wraparound. This is retained as a caution that TCP sequence arithmetic is modular and must be tested across wrap, corroborating existing sequence/wraparound testing guidance rather than being promoted as a standalone rule.
- **!16297 / !16298 — merged master plus stable backport.** DHCP Option 82 suboption 19 is presence-only, not an integer-valued field. This corroborates the existing rule that registered field types must reflect protocol semantics; the master MR is weighted above the backport.
- **!16295 — merged master.** The NMEA GLL dissector arrived with valid/invalid sample traffic and documented fuzzing, while review also preserved GLib-defined boolean types at actual GLib API boundaries. This corroborates existing capture/fuzz testing and C-type boundary guidance.
- **!16290, !16293, !16292, !16289, !16288, !16286, !16282, !16279, !16268, !16266, !16265, !16264, !16263, !16262, !16260, !16259, !16256, and !16249 — mostly merged C99/type-modernization work.** Together they reinforce the existing preference for standard C types in Wireshark-owned code while preserving dependency-defined types where API contracts require them. Existing `c-type-conventions.md` already captures that distinction, so no redundant rule was added.
- **!16294 — open draft/unmerged** and **!16278 — closed/unmerged** were deliberately down-weighted relative to accepted changes. !16278 was retained only for the useful review-process rationale noted above.

Notebook convention commits made during this run:

- `177fef2bcdce6a729f6c5d1b99afcfd007233915` — packet-time terminology convention.
- `0f338b7fefb680a988aaeeb906abb8ec26dcc5c8` — tap-registration and configuration-precedence lifecycle rules.
- `eef7cf3e5c6e788b0e6827f4b0252426105d34a1` — bulk mechanical-refactor reviewability rule.
