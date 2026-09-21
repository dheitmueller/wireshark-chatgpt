# Wireshark MR automation ledger: !14560–!14609

Corpus commit reviewed: `1c7ad509887ee25079a7865cc62ba18cba06f49a`

This run reconstructed the already-reviewed set from the available notebook tracking, including `reviewed-mrs.md`, the aggregate automation ledger, and the per-run files under `reviewed-mrs-automation/`. Selection was by exact MR-number subtraction, not by assuming that an apparent numeric range was completely covered. The historical !17571–!17620 batch remains part of the reviewed set. With the corpus commit unchanged from the prior run, the fifty highest-numbered corpus MRs not already represented in tracking were exactly !14609 through !14560.

Outcome weighting: 46 MRs were merged, !14601/!14600/!14599 were closed without merge, and !14588 was still open in this corpus snapshot. The latter four were therefore treated as weaker evidence than merged work; the three TWAMP attempts were superseded by the already-reviewed accepted fix, and !14588 was retained mainly for John Thacker's review warning about combining TCP PDU desegmentation with weak/default-port heuristics.

## Durable notebook findings

- **!14609 — Deep, merged, John Thacker authored/merged.** JSON's private key-to-decoder callback registry could invoke JSON-3GPP even when that protocol was disabled. The accepted fix resolves the owning protocol and checks `proto_is_protocol_enabled()` before dispatch. Added `protocol-enablement-dispatch-conventions.md`, including the architectural preference for normal dissector-table dispatch where appropriate.
- **!14605 + !14598 — Deep, merged, Martin Mathieson authored/merged.** `check_typed_item_calls.py` learned to parse `string_string` tables, reject duplicate keys, require termination, and require the canonical `{ NULL, NULL }` sentinel. Added structural-table guidance to `checker-target-conventions.md`.
- **!14561 — Deep, merged, John Thacker authored/merged.** Preference/module effect flags are required to be nonzero for mutable preference changes, and `PreferencesChanged` should not be emitted when the accumulated effect/change mask is zero. Added the no-op-notification/effect-metadata rule to `state-refresh-conventions.md`.
- **!14584 + !14589 — Deep/corroborating, merged.** ENRP validates that parameter/error-cause lengths are at least the header size before advancing, preventing zero-length infinite loops. This strongly corroborates the notebook's existing parser forward-progress and malformed-length guidance rather than creating a duplicate rule.
- **!14572 + !14576 — Deep/corroborating, merged.** The GSSAPI fix returns an explicit plaintext subset for signed-only KRB wrap tokens instead of letting callers treat the whole remainder of the original tvb as plaintext; the related HiPerConTracer heuristic change prevents that traffic from being stolen by an unrelated heuristic. The paired capture/testing discussion reinforces precise helper-output contracts and negative heuristic tests.
- **!14588 — Reviewed but down-weighted; open in corpus snapshot.** John Thacker points out that calling `tcp_dissect_pdus()` before a weakly-bound dissector has performed its rejection heuristic can make false-positive classification irreversible once a PDU length has been reported. Useful negative design evidence, but not promoted as accepted convention because this MR was not merged.
- **!14560 — Deep/corroborating, merged.** Auto-generated Decode-As preferences must identify the dissector, not merely the protocol, when multiple described dissectors for one protocol can register in the same table. This is retained as architecture evidence but not promoted separately because later notebook material has stronger coverage of dissector-table identity and binding semantics.

## Exact MRs reviewed

| MR | Review | Outcome / durable observation |
| --- | --- | --- |
| !14609 | Deep | Merged. Generic JSON callback dispatch now honors target-protocol enablement; promoted. |
| !14608 | Scanned | Merged. Documentation include tags made explicit for older supported Asciidoctor behavior; corroborates dependency-version compatibility. |
| !14607 | Scanned | Merged. Debian tooling moves supported Debian releases toward Qt 6 packages. |
| !14606 | Scanned | Merged. Follow-up `debian-setup.sh` packaging/tooling correction. |
| !14605 | Deep | Merged. Checker requires canonical termination of `string_string` tables; promoted with !14598. |
| !14604 | Scanned | Merged. Debian setup/package adjustment in the Qt 6 migration sequence. |
| !14603 | Scanned | Merged. Debian setup switches supported environment to Qt 6. |
| !14602 | Scanned | Merged. Qt 6 dependency/package-list tooling update. |
| !14601 | Down-weighted | Closed, unmerged TWAMP timestamp-format attempt; superseded by accepted later work already reviewed. |
| !14600 | Down-weighted | Closed, unmerged TWAMP timestamp-format attempt; superseded. |
| !14599 | Down-weighted | Closed, unmerged TWAMP timestamp-format attempt; superseded. |
| !14598 | Deep | Merged. Checker detects duplicate `string_string` keys; promoted with !14605. |
| !14597 | Scanned | Merged release-4.2 backport raising sharkd JSON command buffer size; backport evidence only. |
| !14596 | Scanned | Merged backport of preference null-dereference/Coverity cleanup. |
| !14595 | Deep/corroborating | Merged, John Thacker authored/merged. Registers HTTP/2 heuristic directly on TCP as well as the HTTP-specific path; reinforces keeping heuristic scope aligned with actual encapsulation. |
| !14594 | Corroborating | Merged release-4.2 backport of empty JSON-3GPP `suppFeat` guard. |
| !14593 | Deep/corroborating | Merged master fix: do not create a subset from a zero/invalid semantic length even where a field API gives `-1` special meaning; reinforces length-contract guidance. |
| !14592 | Scanned | Merged master preference null-dereference/Coverity cleanup and obsolete-function removal. |
| !14591 | Scanned | Merged MEGACO obsolete-code cleanup; no new reusable convention. |
| !14590 | Scanned | Merged MEGACO statistics-descriptor dissection addition; protocol-specific. |
| !14589 | Corroborating | Merged backport of !14584 malformed ENRP length/forward-progress fix. |
| !14588 | Deep negative evidence | Open/unmerged in corpus snapshot. TCP desegmentation proposal; John Thacker warns that `tcp_dissect_pdus()` can preclude later heuristic rejection and increase false positives. |
| !14587 | Scanned | Merged TCP regression correction restoring expected ACKed-unseen-segment analysis. |
| !14586 | Corroborating | Merged backport of Thrift recursion checking across containers/structures; reinforces centralized/bounded recursion guidance. |
| !14585 | Scanned | Merged Diameter 3GPP Third-Context-Identifier AVP addition; protocol data update. |
| !14584 | Deep/corroborating | Merged, John Thacker authored/merged. Rejects ENRP lengths smaller than their 4-byte header before looping; avoids infinite loop. |
| !14583 | Scanned | Merged GSUP ePDG/PCO protocol coverage; protocol-specific. |
| !14582 | Scanned | Merged Wi-Fi NAN dissector correctness cleanup. |
| !14581 | Deep/corroborating | Merged. Startup interface-list visibility now follows persisted preference state; discussion distinguishes startup list semantics from Manage Interfaces semantics. |
| !14580 | Scanned | Merged. Protobuf UAT maps URI to message type; configuration/protocol feature. |
| !14579 | Corroborating | Merged master sharkd JSON buffer-size increase; primary change corresponding to !14597. |
| !14578 | Scanned | Merged Qt workaround for an upstream Qt regression; corroborates narrowly-scoped dependency-version workarounds. |
| !14577 | Scanned | Merged Qt interface hidden/show-state persistence correction. |
| !14576 | Deep/corroborating | Merged. HiPerConTracer heuristic explicitly rejects LDAP/SASL/KRB wrap traffic demonstrated by the paired capture in !14572. |
| !14575 | Scanned | Merged ASTERIX generator/spec adaptation to upstream structure change; reinforces updating generators/source-of-truth inputs. |
| !14574 | Scanned | Merged removal of unused `PREF_DECODE_AS_UINT`; dead API cleanup. |
| !14573 | Scanned | Merged documentation duplicate-section cleanup. |
| !14572 | Deep/corroborating | Merged, John Thacker authored/merged. GSSAPI returns a plaintext subset for signed-only KRB wrap tokens; tested together with !14576. |
| !14571 | Corroborating | Merged stable backport of CSV hidden-last-column export fix. |
| !14570 | Corroborating | Merged stable backport of the same CSV export fix. |
| !14569 | Scanned | Merged scheduled generated-data update. |
| !14568 | Scanned | Merged scheduled generated-data update. |
| !14567 | Scanned | Merged scheduled generated-data update. |
| !14566 | Scanned | Merged scheduled generated-data update. |
| !14565 | Scanned | Merged master CSV export fix respecting hidden final column. |
| !14564 | Corroborating | Merged master Thrift recursion correction covering nested containers and structures. |
| !14563 | Scanned/corroborating | Merged broad scripted spelling cleanup; reinforces moving repeatable mechanical checks into tooling where practical. |
| !14562 | Scanned | Merged Qt sequence-diagram tooltip improvement for elided comments. |
| !14561 | Deep | Merged, John Thacker authored/merged. Effect flags must describe real preference changes; no `PreferencesChanged` on no-op. Promoted. |
| !14560 | Deep/corroborating | Merged, John Thacker authored and Anders Broman merged. Auto-generated Decode-As preferences use dissector identity where protocol identity is not unique. |

Exact reviewed set for this run: `14609, 14608, 14607, 14606, 14605, 14604, 14603, 14602, 14601, 14600, 14599, 14598, 14597, 14596, 14595, 14594, 14593, 14592, 14591, 14590, 14589, 14588, 14587, 14586, 14585, 14584, 14583, 14582, 14581, 14580, 14579, 14578, 14577, 14576, 14575, 14574, 14573, 14572, 14571, 14570, 14569, 14568, 14567, 14566, 14565, 14564, 14563, 14562, 14561, 14560`.

The corpus is not exhausted: `mr_14559.json` exists at the same corpus commit and is merged. Absent newly scraped higher-numbered unreviewed MRs, !14559 is the next descending candidate.
