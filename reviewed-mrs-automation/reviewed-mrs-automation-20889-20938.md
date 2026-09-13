# Automated Wireshark MR review — !20889 through !20938

Corpus revision: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from all available `reviewed-mrs-automation/` ledgers plus the sparse `reviewed-mrs.md` ledger, preserving the historical !17571-!17620 batch. !20939 was already reviewed in the immediately preceding run, so the fifty highest-numbered corpus MRs not already reviewed are exactly !20938 down through !20889. All fifty IDs are present in this corpus revision.

Weighting: merged master MRs receive the strongest evidentiary weight; authoritative maintainer-authored/reviewed changes receive additional weight. Closed/draft/superseded work is retained in the audit set but not treated as accepted implementation precedent.

## Exact reviewed set

- !20938 — Scanned, merged. John Thacker ws80211 population comments plus earlier propagation of `ws80211_get_phys()` failure; useful clarity/error-path cleanup, no new durable convention.
- !20937 — Deep, merged. Makes allocator scope explicit in `val_to_str()` and updates callers/docs to use scopes such as `pinfo->pool`; strongly corroborates existing explicit allocator-scope guidance.
- !20936 — Discussion-focused, closed draft/superseded. TCP Distal Algorithm experiment; author states it was already handled in !17342. Down-weighted and not used as accepted precedent.
- !20935 — Scanned, merged. Non-user-visible typo cleanup.
- !20934 — Scanned, merged. Automatic data/translation update retry; no durable review lesson.
- !20933 — Scanned, merged. Automatic data/translation update retry; no durable review lesson.
- !20932 — Scanned, merged. Automatic data/translation update; no durable review lesson.
- !20931 — Scanned, merged. TPM typo cleanup.
- !20930 — Scanned, merged. Final `val_to_str` to allocator-aware conversion; corroborates !20937.
- !20929 — Scanned, merged. KNX/IP cleanup and allocator-aware string conversion; no separate durable rule.
- !20928 — Scanned, merged. Moves Wiretap documentation to its relevant documentation location.
- !20927 — Deep, merged; authored/merged by John Thacker. Fixes HE/EHT PHY-capability decoding on big-endian hosts by respecting the actual serialized byte representation rather than using a native-order 32-bit accessor. Promoted to `wire-format-integer-access-conventions.md`.
- !20926 — Scanned, merged. Packet Bytes EBCDIC-to-ASCII viewing support and encoding-selection cleanup.
- !20925 — Discussion-focused, merged; John Thacker. Maps dumpcap 802.11 channel failures to Wireshark `WS_EXIT_*` status domains instead of raw errno/ad-hoc numeric statuses; corroborates existing CLI exit-status conventions.
- !20924 — Scanned, merged. Avoids adding an extra newline before `cmdarg_err()` formatting.
- !20923 — Scanned, merged. GSMTAP whitespace/indentation cleanup.
- !20922 — Scanned, merged. GSMTAP shows otherwise skipped/unused SIM radio fields where appropriate.
- !20921 — Scanned, merged. O-RAN FH CUS ST10 type-5 PRB selection fixes.
- !20920 — Scanned, merged. Adds Falco event conversation fields.
- !20919 — Scanned, merged. Updates the Stratoshark CloudTrail profile.
- !20918 — Scanned, merged. Enables auto-switch filters in global profiles.
- !20917 — Scanned, merged. Falco CloudTrail column/source-address fixes.
- !20916 — Deep, merged; authored by Guy Harris and approved by Anders Broman. Consolidates Wiretap byte-integer access into generic `wsutil/pint.h`, adds one-byte/non-power-of-two-width helpers, uses them for 40-bit timestamps, and removes reliance on transitive includes. Promoted to `wire-format-integer-access-conventions.md`.
- !20915 — Scanned, merged. BLF subtree-registration cleanup.
- !20914 — Scanned, merged. BLF Ethernet PHY State endian correction.
- !20913 — Scanned, merged. Corrects Qt mnemonic handling for plugin menu items.
- !20912 — Scanned, merged. BLF field-formatting/type improvements for timestamps/version data.
- !20911 — Scanned, merged. Adds `hf_frame_encoding` to expose Packet Bytes text-encoding context.
- !20910 — Scanned, merged. Reorganizes 802.11 frequencies/channel-width capabilities by band.
- !20909 — Scanned, merged. BLF byte-buffer access cleanup toward common access macros.
- !20908 — Scanned, merged. BIST-OUCH compiler-visible typo fix.
- !20907 — Scanned, merged. O-RAN FH CUS ST10 report-type-5 corrections.
- !20906 — Scanned, merged. Updates Npcap dependency to 1.83.
- !20905 — Scanned, merged. LI5G associates IMSI when SUPI-IMSI is present.
- !20904 — Scanned, merged. C2P cleanup including allocator-aware value-to-string use and local code organization.
- !20903 — Deep, merged; Michael Mann, approved by Anders Broman. Replaces an inadequately discriminating UDP heuristic (port/size only, no real identifying bytes) with configurable port binding, and adopts `udp_dissect_pdus()` for multiple messages. Promoted to `heuristic-dissector-conventions.md`.
- !20902 — Deep, merged; Michael Mann, approved by Anders Broman. Moves XDLC shared parsing from core `epan` to the dissector layer because it is common protocol dissection rather than generic epan API functionality. Promoted to `epan-layering-conventions.md`.
- !20901 — Scanned, merged. Allocator-aware `val_to_str` conversions split into smaller commits where conversions were not mechanical; useful submission-scope corroboration but no new rule.
- !20900 — Scanned, merged. Batch allocator-aware `val_to_str` conversion.
- !20899 — Scanned, merged. Batch allocator-aware `val_to_str` conversion.
- !20898 — Scanned, merged. Packet Bytes hover highlighting is cleared when the mouse leaves the view while preserving selected-field highlighting.
- !20897 — Scanned, merged. Restores xsltproc installation documentation.
- !20896 — Discussion-focused, merged. BLF writer propagates more specific error information for failed output operations; useful error-reporting corroboration, no separate convention promoted.
- !20895 — Scanned, merged. Stratoshark manual-page corrections.
- !20894 — Scanned, merged. Adds a native Windows Asciidoctor package path in the build/dependency machinery.
- !20893 — Deep, merged. Adds an explicit generated HTTP/3 frame-to-QUIC-stream ID because packet/transport identity cannot recover that mapping when multiple QUIC streams and HTTP/3 frames share packets. Strongly corroborates existing protocol-layer stream-identity guidance.
- !20892 — Scanned, merged. Adds an HTTP/2 body-decompression preference analogous to HTTP/1.
- !20891 — Scanned, merged. Large allocator-aware `val_to_str` conversion batch; corroborates explicit allocation-scope guidance.
- !20890 — Scanned, merged. O-RAN tap suppresses meaningless zero RE/PRB counts for C-plane records.
- !20889 — Scanned, merged; John Thacker. TLS/DTLS exports fallback `data` application payloads as Exported PDUs when no application-data dissector claims them, aligning behavior with TCP/UDP export paths.

## Durable notebook changes from this run

- `wire-format-integer-access-conventions.md` — serialized-width/byte-order access and generic `wsutil/pint.h` ownership, based primarily on !20927 and high-authority Guy Harris MR !20916.
- `epan-layering-conventions.md` — core epan is for generic dissection/framework APIs; protocol-family shared parsing remains in the dissector layer, based on merged !20902.
- `heuristic-dissector-conventions.md` — do not call port/size guessing a heuristic without identifying packet evidence; prefer configurable dispatch when identification is deployment-specific, based on merged !20903.

Existing notebook rules were corroborated rather than duplicated for explicit allocator scopes (!20937 and the conversion series), project-defined CLI exit statuses (!20925), and explicit higher-layer stream identity (!20893).
