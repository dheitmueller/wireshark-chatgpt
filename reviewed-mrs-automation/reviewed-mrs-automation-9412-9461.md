# Automated Wireshark MR review — !9461 through !9412

Review date: 2026-10-31
Corpus: `dheitmueller/wireshark-corpus-mrs`
**Corpus commit:** `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `61571f156d645ddb2b6bc435608219f299035c34`
Review order: highest-numbered previously unreviewed corpus records first; exact membership, not numeric-range assumption.

**Exactly 50 distinct MRs were reviewed: 49 merged, one closed without merging (!9417).** All 50 MR JSON records exist at the pinned corpus commit. Their descriptions, state, discussion data and available per-file diffs were inspected. Generated/binary changes omitted from the archive were not inferred. No build, unit-test or fuzzing execution was performed.

## Prior-review ledger reconciliation

At notebook start, the GitHub tree exposed 357 files under `reviewed-mrs-automation/` plus root `reviewed-mrs.md`. Inventoried every path, including exceptional noncontiguous/skip/plus/gap ledgers. Inspected the aggregate and supplemental reviewed sets, adjacent prior run !9511–!9462, exceptional !9963–!10312 and other gap tracking. The newest existing lower-numbered batch ends at !9462, and neither aggregate ledger identifies any !9412–!9461 as reviewed. The previous per-run ledger contains exactly 50 distinct rows !9511–!9462 and identifies !9461 only as its next candidate. All 50 candidate corpus records are present. Higher-numbered corpus records are already accounted for by the existing tracking, including the named exceptional/backfill files; no ranges were blindly promoted to reviewed without membership evidence.

**Preserved historical !17571–!17620:** read `reviewed-mrs-automation-17571-17620.md` and counted **50 unique explicitly reviewed table entries, one per MR !17571 through !17620**. This ledger was not modified.

## Exact reviewed MR set

| MR | Outcome | Depth | Evidence and assessment |
| --- | --- | --- | --- |
| !9461 | merged | Scanned | Release-4.0 backport of !9459: ElidedLabel styles via setSmallText rather than escaped HTML. |
| !9460 | merged | Scanned | Release-4.0 backport of !9458: initialize Expert Info filter checkbox only on dialog creation. |
| !9459 | merged | Deep | John Thacker master fix: IO Graph and Sequence hints use ElidedLabel styling API, not rich HTML. |
| !9458 | merged | Deep | John Thacker master fix preserves manual unchecking of display-filter limit during UI refresh. |
| !9457 | merged | Scanned | Release-3.6 backport of !9455 for ExpertInfoDialog hint text. |
| !9456 | merged | Scanned | Release-4.0 backport of !9455 for ExpertInfoDialog hint text. |
| !9455 | merged | Deep | ElidedLabel setSmallText replaces HTML tags that would be escaped; master implementation. |
| !9454 | merged | Scanned | Release-3.6 LwM2M TLV resource-ID lookup backport of !9448. |
| !9453 | merged | Scanned | Release-4.0 LwM2M TLV resource-ID lookup backport of !9448. |
| !9452 | merged | Deep | John Thacker: Protocol Hierarchy stats guard capture-file read_lock, disable while rescan, and wire Stop to cf->stop_flag. |
| !9451 | merged | Discussion-focused | Gilbert Ramirez requested conflict resolution; Jaap Keuter directed contributor to omit auto-generated enterprise.tsv and fix make-sminmpec.py only. |
| !9450 | merged | Scanned | Release-4.0 backport of !9438: sort typed GeoIP text by unformatted model role, not canConvert<double>. |
| !9449 | merged | Scanned | Release-4.0 backport of !9437 for displayed-to-canonical conversation column indexes. |
| !9448 | merged | Deep | Stig Bjørlykke replaces ambiguous synthesized LwM2M URI path with explicit object and resource identifiers from correct TLV/CoAP sources. |
| !9447 | merged | Deep | John Thacker: Qt typed QVariant filters including GeoIP strings, floats, rounded percentages and QTime/QDateTime on Qt 5/6. |
| !9446 | merged | Scanned | João Valverde MinGW warning cleanup: platform address types, GetProcAddress diagnostic scoping, prototypes and CMake flags. |
| !9445 | merged | Scanned | Release-3.6 AppleClang warning-version-gate backport of !9440. |
| !9444 | merged | Scanned | Release-4.0 AppleClang warning-version-gate backport of !9440. |
| !9443 | merged | Scanned | Release-4.0 backport of welcome-page donation link !9439. |
| !9442 | merged | Scanned | Release-3.6 AppStream screenshot link stabilization backport of !9433. |
| !9441 | merged | Scanned | Release-4.0 AppStream screenshot link stabilization backport of !9433. |
| !9440 | merged | Deep | Gerald Combs narrowly adjusts AppleClang compiler-version diagnostic gate for false missing-braces warnings in versions 10 and 11. |
| !9439 | merged | Scanned | Welcome page donation link; product-resource change without new general coding convention. |
| !9438 | merged | Deep | John Thacker: QVariant canConvert<double> tests type-level capability, not actual numeric string conversion; sort by proper data role. |
| !9437 | merged | Deep | John Thacker: map proxy filter columns correctly when Conversation ID and total columns are hidden. |
| !9436 | merged | Deep | Guy Harris cites C11 isinf contract: any nonzero means infinity; never require exactly +1 or -1. |
| !9435 | merged | Scanned | Release-3.6 backport of CoAP/RTPS aggregate initialization fix !9431. |
| !9434 | merged | Scanned | Release-4.0 backport of CoAP/RTPS aggregate initialization fix !9431. |
| !9433 | merged | Scanned | AppStream screenshots use maintained User Guide graphics and captions instead of personal web paths. |
| !9432 | merged | Deep | Alexis La Goutte reviews Short SSID byte order, related field coverage, wrong endian encoding on Boolean field, and subject length. |
| !9431 | merged | Deep | Gerald Combs initializes CoAP/RTPS values after Valgrind reports; post-merge GCC 4.8 failure led to !9529. |
| !9430 | merged | Scanned | OBD-II Service 0x04 request/response has zero parameter bytes; do not apply incompatible generic length minimum. |
| !9429 | merged | Deep | WebSocket follower queues unmasked post-framing payload via tap; historical hardcoded enum/UI pattern later superseded by !9957. |
| !9428 | merged | Scanned | João Valverde fixes MinGW printf integer widths and expands nonportable strftime format shorthands. |
| !9427 | merged | Scanned | Cleans up version-info punctuation and debug-build output. |
| !9426 | merged | Scanned | Release-4.0 backport of !9419: UAT append checks rowData against columnCount, not rowCount. |
| !9425 | merged | Scanned | Mongo BSON Decimal128 recognized, honestly displayed as bytes until semantic converter is implemented; Martin Mathieson approved. |
| !9424 | merged | Deep | Martin Mathieson adds eCPRI generated length and first-pass O-RAN eAxC flow state with C/U-plane frame references. |
| !9423 | merged | Discussion-focused | MP4 segment styp/sidx support and samples; Anders Broman queried whether 0/1 reference type should be Boolean; enum retained. |
| !9422 | merged | Scanned | Release-3.6 BGP-LS IPv6 Neighbor Address size validation uses length constant, not TLV type constant. |
| !9421 | merged | Scanned | Release-4.0 BGP-LS IPv6 Neighbor Address size validation companion fix. |
| !9420 | merged | Deep | AppImage packaging switches to linuxdeploy and Qt plugin, with CMake prerequisites; archived AppRun binary diff unavailable. |
| !9419 | merged | Deep | John Thacker fixes UatModel appendEntry dimension check: row data may be shorter than columns, not longer. |
| !9418 | merged | Discussion-focused | Pascal Quantin directs generated AUTHORS email fix to .mailmap and asks to squash corrective history; merged as one change. |
| !9417 | closed-unmerged | Discussion-focused | Release-3.6 nested dependency attempt closed; do not treat as accepted fix or repeat !9414 without !9538 and !9622. |
| !9416 | merged | Deep | TCP stream identity/base sequence initialized independently of optional TCP Sequence Number Analysis; release-4.0. |
| !9415 | merged | Scanned | Release-3.6 backport of !9416 for reused 5-tuples with sequence analysis disabled. |
| !9414 | merged | Deep | Sake Blok nested frame dependencies; user reproducer revealed recursive hangs; John Thacker insisted !9538 and !9622 accompany backport. |
| !9413 | merged | Scanned | Adds GitHub FUNDING.yml donation metadata. |
| !9412 | merged | Scanned | João Valverde MSYS2 workflow uses package PKGBUILD via makepkg-mingw and publishes package artifact. |

## Durable conclusions and evidentiary weighting

- **Qt semantic types and user state (!9447, !9438, !9437, !9458):** sorting, filtering and hidden-column mapping depend on canonical typed model roles and user choices; do not coerce arbitrary strings to numbers or reset checkbox choices on routine refresh.
- **Authoritative review (!9436, !9451, !9418, !9414):** Guy Harris establishes the standard-defined nonzero `isinf` contract; Jaap Keuter says to omit automatically regenerated enterprise TSV; Pascal Quantin identifies `.mailmap` as the source of AUTHORS data and requests squash; John Thacker insists dependency-graph backports include their recursion/performance follow-up fixes.
- **Layer transformations (!9429):** the WebSocket follower must consume unmasked payload, not masked TCP bytes. Its hard-coded Follow enum is **historical**, superseded by later !9957 registration-based design.
- **Stateful correctness (!9416, !9452):** TCP identity must exist even with optional sequence analysis disabled; capture-file scans must own a read lock, progress and cancellation.
- **Merged does not imply complete (!9431, !9414):** GCC 4.8 compatibility issues and real high-CPU recursive hangs surfaced after the initial merge, prompting !9529, !9538 and !9622. The later corrective fixes have stronger architectural weight. Closed !9417 is down-weighted.
- **Multiple branch backports:** !9461/!9459, !9457/!9456/!9455, !9454/!9453/!9448, !9445/!9444/!9440, !9435/!9434/!9431, !9426/!9419, !9416/!9415 and others represent related changes, not independent policy evidence.

## Next run

Next expected candidate: **!9411**, subject to rebuilding exact reviewed membership if the corpus or notebook changes. Corpus not exhausted. **Do not pause or disable the recurring automation.**
