# Wireshark MR review ledger: !20389–!20438

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from all available `reviewed-mrs-automation/` ledgers plus `reviewed-mrs.md`, preserving and counting the historical !17571–!17620 batch. Selected the 50 highest-numbered corpus MRs not present in that set; no numeric-range inference was used. Every selected corpus object exists at the pinned commit.

Review direction: newest to older.

Exact reviewed MRs (50):

- !20438 — merged — scanned; NR RRC generated/specification update.
- !20437 — merged — discussion-aware; 802.11ax radio-info addition, with later static-analyzer warning noted.
- !20436 — merged — scanned; LTE RRC generated/specification update.
- !20435 — merged — deep; DTLS 1.2 reassembly identity must include epoch because `message_seq` resets on renegotiation. Promoted to `stateful-reassembly-conventions.md`.
- !20434 — merged — deep, Guy Harris-authored; candump probing deliberately avoids interface-table/IDB side effects before file ownership is established. Promoted to `wiretap-file-detection-conventions.md`.
- !20433 — merged — scanned; NRPPa generated/specification update.
- !20432 — merged — scanned; `find_filter_matches.py` capture-search utility.
- !20431 — merged — scanned; NAS 5GS specification update.
- !20430 — merged — deep/corroborating, Guy Harris-authored; candump maps real interface names to per-file interface IDs/IDBs. Reinforces Wiretap interface-identity guidance.
- !20429 — merged — scanned; GSM SIM extended-length APDU support.
- !20428 — merged — discussion-aware; late DSB metadata marks files modified and can require redissection because newly available secrets affect earlier packets.
- !20427 — merged — scanned; WiMAX array-bound fixes from Coverity.
- !20426 — merged stable backport — scanned; ISOBUS display correction.
- !20425 — merged stable backport — corroborating; Gryphon remaining-length underflow fix.
- !20424 — merged stable backport — corroborating; same Gryphon underflow fix.
- !20423 — merged — scanned; ISOBUS display correction.
- !20422 — merged — corroborating, John Thacker-authored; compute remaining length at the advanced offset rather than subtracting from an earlier unsigned value.
- !20421 — merged — discussion-aware; PFCP enterprise IE review explicitly accounts for protocol-defined zero-length semantics.
- !20420 — merged — corroborating, John Thacker-authored; MATE runtime mutations belong in runtime data, not parsed longer-lived configuration.
- !20419 — merged — deep, John Thacker-authored; zero-length byte arrays may validly have a NULL source pointer and conversion APIs should handle the empty case before pointer assertions.
- !20418 — merged stable backport — scanned; GSM SIM optional P3 handling.
- !20417 — merged — discussion-aware; pipeline diagnostics caught invalid `ENC_NA` use for integer fields; accepted revision uses explicit endian encodings.
- !20416 — merged stable backport — corroborating, Guy Harris-authored; per-interface timestamp-type strings need independent ownership.
- !20415 — merged stable backport — corroborating, Guy Harris-authored; same ownership fix.
- !20414 — merged — deep; John Thacker review establishes per-pass transient SMB state and redissection-safe handling instead of static frame-number history. Promoted to `dissector-state-conventions.md`.
- !20413 — merged — deep, Guy Harris-authored/merged; mutable per-interface option strings must copy shared defaults rather than aliasing storage later freed by an instance. Promoted to `memory-lifetime-conventions.md`.
- !20412 — merged — scanned; O-RAN SE24 offset/config-count correction.
- !20411 — merged — corroborating; fuzz-discovered RTPS NULL dereference fixed with direct nullable-result validation.
- !20410 — merged — discussion-aware; 802.11bn Trigger Basic addition; maintainer review included style cleanup.
- !20409 — merged stable backport — scanned; GSM SIM unknown-command Le detection.
- !20408 — merged stable backport — scanned; truncation-label formatting correction.
- !20407 — merged — scanned; GSM SIM optional P3 handling.
- !20406 — merged — scanned; protocol-tree truncation marker placement.
- !20405 — merged — scanned; truncation-label formatting correction.
- !20404 — merged — corroborating, John Thacker-authored; use remaining-length APIs to avoid signed/unsigned overflow traps and honor `ftell()` error returns.
- !20403 — merged — corroborating; MATE owned AVP cleanup.
- !20402 — merged stable backport — discussion-aware; column registration changes require rebuilding affected column state on Lua reload.
- !20401 — closed draft/superseded — down-weighted; abandoned SMB approach superseded by merged !20414.
- !20400 — closed draft — down-weighted; unmerged ILNP dissector work not treated as accepted precedent.
- !20399 — merged — scanned; Qt coloring-rule checkbox palette correction.
- !20398 — merged — corroborating, Gerald Combs-authored; static analysis caught a loop variable narrower than its upper bound.
- !20397 — merged — scanned/testing-aware; sharkd load limits exposed through JSON-RPC with focused tests.
- !20396 — merged — scanned; ACDR registration corrected to UDP-only.
- !20395 — merged — scanned; O-RAN SE24 presentation cleanup.
- !20394 — merged — scanned; TPNCP bitfield display corrected for little-endian source ordering.
- !20393 — merged — scanned; TPNCP generated-data sorting cleanup.
- !20392 — merged — scanned; TPNCP 64-bit field support.
- !20391 — merged — scanned; TPNCP protects illegal non-NULL enum strings on IP field types.
- !20390 — merged — scanned; GSM SIM common response-APDU handling.
- !20389 — merged — corroborating, John Thacker-authored/merged; runtime-created MATE PDUs are owned/freed as runtime state rather than mutating parsed configuration.

Notebook changes promoted from this batch:

1. `wiretap-file-detection-conventions.md` — speculative probes must not create persistent reader state before format ownership is established (!20434).
2. `memory-lifetime-conventions.md` — independently mutable objects must own copies of shared default strings when their replace/free lifecycle is independent (!20413; stable backports !20415/!20416).
3. `dissector-state-conventions.md` — state used only to coordinate repeated dissector calls within one dissection pass should have per-pass lifetime; GUI redissection must start clean (!20414).
4. `stateful-reassembly-conventions.md` — include epoch/generation in reassembly identity when the protocol resets its sequence namespace (!20435).

Strong corroboration retained without duplicating notebook rules: !20430 (real capture-interface identity), !20422/!20425/!20424 (remaining-length arithmetic), !20420/!20389 (configuration vs runtime ownership), !20419/!20421 (legitimate zero-length semantics), !20417 (explicit field encodings), !20411 (fuzz/nullability), !20404 (bounds/error-return handling), and !20398 (static-analysis type-width warning).
