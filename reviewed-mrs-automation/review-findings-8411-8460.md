# Review findings: !8411–!8460

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This batch contains 48 merged MRs and two closed/unmerged MRs (!8453 and !8411). Closed or superseded work is retained only as lower-weight design/history evidence.

## Durable findings

- **Nested framing context (!8456, !8455).** MACsec and TECMP both demonstrate that an embedded Ethernet dissector cannot safely infer its semantic frame length from the enclosing capture. The parent that knows the embedded object's true geometry must supply that context for the child call and restore outer packet state afterward.
- **Display-filter language design (!8444, !8431).** A recently introduced `<...>` literal syntax collided with ordinary relational expressions such as `a < b or a > c`. João Valverde removed the experimental syntax, documented the removal, and added a regression test rather than reserving another delimiter speculatively.
- **Header and library layering (!8443, !8442).** Generic hexdump/EBCDIC functionality moved to wsutil, while wmem stopped pulling in the broad `wireshark.h` umbrella and directly included the standard headers that define its types.
- **Text semantics (!8441, !8434).** Diagnostic presentation should preserve a valid UTF-8 prefix as Unicode and isolate malformed bytes. GIOP additionally shows that wire octet length and converted UTF-8 length are separate coordinate systems and that a protocol-mandated default charset is preferable to treating raw bytes as text when negotiated state is unavailable.
- **Startup lifecycle (!8446, !8432).** Logging APIs have real initialization order: `ws_log_init()` must precede argument parsing, and configuration needed during early logging cannot depend on the much-later preferences subsystem. !8432's later !8482 correction also shows that local machine state can mask migration/key-name mistakes.
- **Object-specific dispatch (!8435).** CIP service opcode alone is not a sufficient semantic key when service formats differ by object class. The accepted implementation uses a (class, service) lookup and lets object-specific handling override generic behavior.
- **Partial initialization cleanup (!8419, !8427, !8429).** Making an optional analysis path disabled by default exposed cleanup that unregistered taps that had never registered. The fix records successful registrations, removes only those listeners, and clears the flags afterward.
- **Static-analysis review (!8422, !8421).** Clang dead-store and field-metadata checkers caught semantic copy/paste bugs rather than cosmetic warnings. Alexis La Goutte explicitly asked Pascal Quantin to validate the NAS fix, while Martin Mathieson surfaced the GTPv2 duplicate-filter warning.
- **Expert-info versus Boolean fields (!8412 → !8473).** Guy Harris's type assertion made a latent TRANSUM misuse visible: `tcp.analysis.*` expert fields are presence markers, not Boolean-valued fields. Already-reviewed !8473 remains the authoritative final rule.

## Per-MR inventory

| MR | Outcome | Depth | Notes |
|---|---|---|---|
| !8460 | merged | Scanned | Automatic generated-data/docs/translation update; no distinct convention. |
| !8459 | merged | Scanned | Automatic manufacturer/services/TLS data update; no distinct convention. |
| !8458 | merged | Scanned | Automatic release-4.0 generated-data/docs/translation update; no distinct convention. |
| !8457 | merged | Scanned | Capture-control icon refresh; UI asset maintenance. |
| !8456 | merged | Deep | MACsec supplies nested Ethernet with effective frame geometry and restores the outer packet length; early framing-boundary evidence. |
| !8455 | merged | Deep | TECMP temporarily supplies embedded Ethernet length through packet context and restores it after the child call. |
| !8454 | merged | Scanned | Logray icon/package artwork refresh. |
| !8453 | closed | Discussion-focused | Guy Harris explicitly abandoned this backport in favor of João Valverde's merged backport; down-weighted. |
| !8452 | merged | Scanned | Development-branch Debian package jobs stop treating expected symbol-list changes as fatal. |
| !8451 | merged | Scanned | Debian symbol metadata follows functions moved from libwireshark to libwsutil. |
| !8450 | merged | Scanned | Stable backport of the display-filter big-endian integer comparison fix represented by !8449. |
| !8449 | merged | Scanned | Fixes a copy/paste field-selection error in big-endian display-filter integer comparison. |
| !8448 | merged | Scanned | Stable backport of RTP Player Qt 6 Windows timing/race fix represented by !8436. |
| !8447 | merged | Scanned | Stable backport of conversation-dialog A/B column-label correction. |
| !8446 | merged | Deep | Makes logging initialization ordering observable and asserts ws_log_init() precedes logging-argument parsing. |
| !8445 | merged | Scanned | Stable backport of sshdump capture-filter propagation. |
| !8444 | merged | Deep | Removes a new display-filter literal syntax after it conflicts lexically with relational operators; adds a targeted regression test and release-note documentation. |
| !8443 | merged | Discussion-focused | Moves generic hexdump/EBCDIC helpers down to wsutil; reinforces library layering. |
| !8442 | merged | Deep | Removes umbrella-header dependency from wmem and directly includes the standard types it uses. |
| !8441 | merged | Deep | Logging preserves valid UTF-8 as Unicode and hex-escapes malformed bytes; marker accounting follows characters rather than continuation bytes. |
| !8440 | merged | Scanned | Conversation-dialog B-to-A column label correction. |
| !8439 | merged | Scanned | wifidump explicitly requests monitor-mode control/other-BSS frames for drivers that do not imply them. |
| !8438 | merged | Scanned | Falcodump profile/region default handling. |
| !8437 | merged | Deep | Editable selector distinguishes a selected item's underlying value from free-form visible text. |
| !8436 | merged | Discussion-focused | RTP Player Qt 6 Windows race fix; review also enforced valid Git author identity and guided stable-branch submission. |
| !8435 | merged | Deep | CIP service dispatch uses (class, service), not service opcode alone, because formats are object-specific; sample capture supplied. |
| !8434 | merged | Deep | John Thacker converts GIOP strings to valid UTF-8 using the protocol default charset when negotiated code-set state is unavailable; wire octet length remains distinct from decoded UTF-8 length. |
| !8433 | merged | Scanned | Debian symbol metadata maintenance after library changes. |
| !8432 | merged | Deep | Moves Windows console-open state to a storage/API available during early logging initialization; later !8482 corrected a key-name mismatch. |
| !8431 | merged | Scanned | Stable backport of !8444's display-filter literal-syntax removal. |
| !8430 | merged | Scanned | Stable backport of f5ethtrailer expensive-analysis preference default represented by !8419. |
| !8429 | merged | Scanned | Stable backport of registered-tap cleanup fix represented by !8427. |
| !8428 | merged | Scanned | sshdump forwards the user's capture filter to remote tcpdump. |
| !8427 | merged | Deep | Tracks successful tap registrations and unregisters only resources actually registered, resetting state during cleanup. |
| !8426 | merged | Scanned | OCP.1 response/request forward lookup field addition. |
| !8425 | merged | Scanned | Restores a WiMAX ASN-CP message-type field lost to an old copy/paste/unused-field mistake. |
| !8424 | merged | Scanned | SharkFest welcome-screen update. |
| !8423 | merged | Scanned | Stable Debian Breaks/Replaces packaging correction. |
| !8422 | merged | Discussion-focused | Alexis La Goutte used a Clang dead store to uncover a semantic length typo, then asked Pascal Quantin to confirm the protocol fix. |
| !8421 | merged | Discussion-focused | Pipeline checker caught a duplicate consecutive filter name with mismatched labels; Martin Mathieson surfaced the copy/paste error and contributor corrected it. |
| !8420 | merged | Scanned | Master Debian Breaks/Replaces correction for headers moved to libwsutil-dev. |
| !8419 | merged | Discussion-focused | Disabling expensive f5ethtrailer analysis by default exposed cleanup that assumed taps were always registered; !8427 fixes it. |
| !8418 | merged | Scanned | Debian symbol metadata maintenance. |
| !8417 | merged | Scanned | 3GPP GPRS CDR documentation-only update. |
| !8416 | merged | Discussion-focused | Adds documented extcap editselector type so known choices can coexist with user-entered overrides; !8437 fixes selected-value semantics. |
| !8415 | merged | Scanned | Stable backport of TRANSUM Boolean extractor fix represented by !8412. |
| !8414 | merged | Scanned | Stable backport of TRANSUM Boolean extractor fix represented by !8412. |
| !8413 | merged | Scanned | Acknowledgements HTTPS correction; supersedes closed !8411. |
| !8412 | merged | Deep/caution | Guy Harris fixes true FT_BOOLEAN storage access, then a post-merge assertion exposes that TCP analysis expert fields have no Boolean value; !8473 is the authoritative presence-based correction. |
| !8411 | closed | Scanned | Superseded acknowledgements URL edit; down-weighted in favor of merged !8413. |
