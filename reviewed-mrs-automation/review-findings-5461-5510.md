# Review findings !5461-!5510

Corpus: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 MRs were reviewed. Merged work carries primary weight; !5506 and !5475 are closed, and !5489 is open, so those three are lower-weight history.

- !5510 merged — WSDG dependency/link cleanup; successor to !5506.
- !5509 merged — encoding/pre-commit cleanup; concrete text encodings no longer combine with `ENC_NA`.
- !5508 merged — project formatting migration to Wireshark allocation wrappers.
- !5507 merged — checker enforcement prevents reintroduction of the old formatting helper.
- !5506 closed — superseded WSDG submission.
- !5505 merged — dissector formatting migration.
- !5504 merged — non-dissector formatting migration.
- !5503 merged — fixes macOS-only declaration/header regression from the migration.
- !5502 merged — fixes duplicate 5co-legacy display-filter abbreviation.
- !5501 merged — automatic data/translation update.
- !5500 merged — release-3.6 automatic update.
- !5499 merged — release-3.4 automatic update.
- !5498 merged — Stig Bjørlykke catches `SCN*` versus `PRI*` format-macro misuse.
- !5497 merged — broad formatted-I/O refactor; macOS conditional build exposed missing header.
- !5496 merged — text import SCTP fragmentation/sequence handling.
- !5495 merged — capability-gated `vasprintf()` optimization.
- !5494 merged — Windows MR CI returns to official CMake after a build-system race.
- !5493 merged — developer documentation example fix.
- !5492 merged — exposes wmem through the umbrella header.
- !5491 merged — wmem string performance tests.
- !5490 merged — indentation cleanup.
- !5489 open — extcap compatibility/IPC discussion; retained only as provisional history.
- !5488 merged — heuristic-dissector documentation maintenance.
- !5487 merged — tapping documentation maintenance.
- !5486 merged — JPEG EXIF value correction.
- !5485 merged — EPUB documentation warning cleanup.
- !5484 merged — John Thacker unifies text2pcap with the shared text-import engine.
- !5483 merged — release-3.6 backport of direction detection.
- !5482 merged — fixes direction-marker/timestamp parsing order.
- !5481 merged — release-3.6 timestamp fallback-tick backport.
- !5480 merged — Windows documentation naming refresh.
- !5479 merged — ccache for the minimal-feature CI job.
- !5478 merged — fixed-width C integer documentation.
- !5477 merged — CI variable fix.
- !5476 merged — release-3.6 radiotap zero-length PPDU fix.
- !5475 closed — accidental 242-commit backport; redone cleanly later.
- !5474 merged — DCT2000 header presentation without payload dissector.
- !5473 merged — standard C11 `_Noreturn` with C++ fallback.
- !5472 merged — Coverity hash-expression fix.
- !5471 merged — Qt/CMake prefix-path update.
- !5470 merged — text-import wording cleanup.
- !5469 merged — synthetic timestamp tick matches output microsecond resolution.
- !5468 merged — earlier Windows CMake CI change, later partly superseded by !5494.
- !5467 merged — release-3.6 complete timestamp-field parsing fix.
- !5466 merged — dummy IDBs serialize non-default timestamp resolution metadata.
- !5465 merged — BLF serializes nanosecond timestamp resolution metadata.
- !5464 merged — removes redundant feature-off CI variant.
- !5463 merged — adds broad optional-feature-off MR build.
- !5462 merged — stable release-note update.
- !5461 merged — IuUP CRC fix adopts the common checksum tree API after John Thacker review.
