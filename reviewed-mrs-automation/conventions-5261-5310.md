# Durable conventions from Wireshark MRs 5261-5310

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly !5310 through !5261. Merged master work is primary evidence; closed !5306, !5291, !5289, and !5277 are lower-weight history.

## Parser helper errors must be unambiguous to the caller

Merged master !5280 fixes a BT-DHT endless loop. On an invalid compact-node length, the helper previously returned `tvb_reported_length_remaining()`, a positive value the caller interpreted as successful consumption. The accepted code returns `0`, causing the caller to report the error and stop. Release backports !5281 and !5282 corroborate the fix.

A parser helper whose return value controls outer-loop advancement must distinguish successful consumption from structural failure. Do not return a convenient positive length on an error path merely because it reaches the end of the helper; the caller may use that value as permission to continue.

## Field display metadata is type-dependent policy, not always a numeric base

Merged !5283 removes `STR_ASCII` and `STR_UNICODE`. During review, Guy Harris points out that the relevant `header_field_info` member is named `display`; “base” is a historical artifact from when only numeric fields had alternate display bases. Jaap Keuter argues for the documented generic neutral value, `BASE_NONE`, and the accepted change uses it for string fields.

Interpret the display member according to the registered field type and supported display modifiers. Do not infer numeric-base semantics from historical `BASE_*` naming, and do not invent string-specific display values when the generic neutral display already expresses the contract.

## Preserve source spelling separately from semantic operators

Merged !5287 stores display-filter lexical token values in the syntax tree so user-facing errors can reproduce the syntax actually typed. An expression written with `&&` should not produce a diagnostic rewritten with the canonical word `and`. Merged !5302 complements this by preserving valid UTF-8 in filter representations instead of escaping each non-ASCII byte.

For a user-facing language, the AST may need both a canonical semantic operator/value and the original lexeme. Use the semantic form for compilation and the source form for diagnostics. Escape text only where the language representation requires it; do not mangle otherwise valid UTF-8.

## Reviewable patch series are independently clean

Merged !5303 contains unusually explicit Jörg Mayer submission guidance. He asks for behavior additions to be separated from foundational format changes, requires whitespace/build fixes to be folded back into the commit that introduced them, asks for representative captures and decoding instructions, and states that the resulting small patches should be easy to review and test. He also recommends stabilizing the architectural foundation before opening dependent feature work to avoid rework.

A meaningful multi-commit MR need not be flattened blindly, but each commit should be internally buildable/check-clean and represent a reviewable logical step. Fixup commits that only repair a defect introduced earlier in the same series should normally be folded into that introducing commit before review/merge. Establish architectural foundations before stacking large dependent features.

The SSH implementation itself later required important correctness fixes, including the already-reviewed !5801 redissection/state separation. Those later fixes are authoritative for runtime semantics; !5303 is promoted here only for its durable review/submission guidance.

## Externally serialized identifier spaces require registry coordination

The same !5303 introduced an SSH pcapng Decryption Secrets Block type. John Thacker noticed that the type was not then documented in the pcapng draft and asked that the format/type be described and registered rather than treated as a locally invented identifier. He subsequently submitted the draft update himself.

When Wireshark writes an externally serialized format whose identifier namespace is maintained by a specification/registry, adding a locally convenient identifier is not sufficient. Coordinate the assigned value and format with the authoritative registry/specification so captures remain interoperable and future assignments cannot collide.

## Dissector state and loop-control types must outlive packet-local accidents

The new ZBOSS dissector in merged !5301 received several durable review corrections. Jaap Keuter rejected a function-static mutable context string because packets can be revisited out of order and multiple streams can be interleaved; such stream-specific state belongs in conversation/state facilities. Gerald Combs also requested changing `guint8` loop counters to `guint`/plain integer types: even when today's packet count is one byte wide, future loop conditions or extensions can overflow a narrow in-memory counter and create very large or infinite loops.

The same review strongly corroborates existing `FT_BOOLEAN` registration rules: a nonzero display value denotes the containing bit-field width, while the mask identifies the bit; `BASE_NONE` is the neutral choice when no packet bit is associated with a generated item.

## UI parity means interaction semantics, not only visual parity

Merged !5295 adds an extcap configuration control to Capture Options. Guy Harris tested the corresponding Welcome-screen and Capture Options behaviors on macOS and Ubuntu and focused on what double-click actually does for normal versus extcap interfaces, not just whether both surfaces display a configuration icon.

When the same capability appears in multiple UI surfaces, compare the complete interaction contract—activation gesture, mandatory-configuration path, default action, and platform behavior—not merely labels/icons. If the surfaces intentionally differ, make that difference explicit rather than accidentally inheriting it from duplicated code.

## Tests for sign-sensitive conversion need an independent oracle

Merged !5298 and !5299 added ISO-8601 basic-format parsing and tests, and !5308 then used that parser for ASN.1 GeneralizedTime. The timezone-offset sign logic and expected test values shared the same misconception; the later already-reviewed !5668, authored by John Thacker, corrected both implementation and vectors.

This batch therefore strengthens the existing time-parsing rule negatively: expected values for sign-sensitive conversions must be derived from independently known instants or another trustworthy oracle, not from the same transformation logic or intuition as the implementation.

## Generated output and authoritative input move together

Closed !5289 proposed an Asterix generator-input correction without the regenerated dissector. Alexis La Goutte asked that the generated output be updated at the same time; Graham Bloice additionally noted that closing/reopening a new MR unnecessarily breaks review continuity. The follow-up !5306 touched both input and output but also closed unmerged.

Because those MRs did not merge, they are lower-weight evidence only, but they corroborate the much stronger existing notebook rule: change authoritative generator input, regenerate committed artifacts, and keep the review thread intact where practical.

## Real-device integrations need lifecycle tests, not only happy-path capture

Merged !5261 extends ciscodump across IOS, IOS XE, and ASA. Beyond Windows portability fixes, Dario Lombardo requested independent testing on real equipment and the contributor enumerated the relevant scenarios: configure/unconfigure the device, capture correctness, sustained load, remote early stop/full buffer, and a user stopping capture from Wireshark.

For extcap or other integrations that mutate external-device state, validation should include cleanup and interruption paths. A successful happy-path capture does not prove the device is left in a safe/clean state when the remote endpoint stops early or the local user aborts.
