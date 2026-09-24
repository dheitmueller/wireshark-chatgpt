# Wireshark MR automation review: !11712 through !11663

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook starting commit: `aa8447e876f1eb6047240f3d4d7f3b801ee11916`
- Review direction: descending MR number, newest available previously-unreviewed first
- Exact number reviewed this run: **50**
- Disposition: **49 merged, 1 closed/unmerged (!11679)**
- Historical reviewed batch **!17571-!17620 is preserved and contributes 50 MR numbers to the reconstructed already-reviewed set.**

## Selection / duplicate-avoidance

Before selecting this batch, the existing `reviewed-mrs.md` tracking and the per-run files under `reviewed-mrs-automation/` were consulted and reconciled. The immediately preceding authoritative ledger explicitly ended at !11713 and treated !11712 only as a frontier probe, not as reviewed. The selected set below was checked as individual MR numbers against the available tracking; numeric interval coverage was not inferred from a partial ledger.

Exact selected set, in review order:

`!11712, !11711, !11710, !11709, !11708, !11707, !11706, !11705, !11704, !11703, !11702, !11701, !11700, !11699, !11698, !11697, !11696, !11695, !11694, !11693, !11692, !11691, !11690, !11689, !11688, !11687, !11686, !11685, !11684, !11683, !11682, !11681, !11680, !11679, !11678, !11677, !11676, !11675, !11674, !11673, !11672, !11671, !11670, !11669, !11668, !11667, !11666, !11665, !11664, !11663`

## Durable notebook changes

1. `capture-linktype-conventions.md` — commit `86373582195c6a1ecb72fcfcbc0f7c339b106738`
   - From Guy Harris's merged master !11672 and release-4.0 backport !11673: `DLT_*` values obtained from libpcap are host/API-domain identifiers and are not universally numerically identical to portable capture-file `LINKTYPE_*` values. Normalize them before writing pcap/pcapng metadata, including at externally piped pcap boundaries where the producer may not have done the conversion.

2. `platform-api-portability-conventions.md` — commit `0474993ca365a79414767abc5089bd860b92bdc3`
   - From Guy Harris's !11664/!11668: standards membership does not prove a facility exists on every supported Unix; gate optional system functionality on the actual capability such as `RLIMIT_AS`, and report the native failure reason when the call fails.
   - From Guy Harris's !11665/!11671: similar-looking external API structs are not guaranteed to be the same C type; copy required members rather than relying on whole-structure assignment when type identity is not promised.

3. `typed-item-checker-conventions.md` — commit `a81b0a69e753fea16a53b82bd437dcb7a551bf0d`
   - From Martin Mathieson's !11689: run `check_typed_item_calls.py --check-bitmask-fields` in normal CI.
   - From merged !11698: source checkers must model valid proto-tree idioms precisely; an all-set mask is valid in the narrow case where it is the first and sole field in an `add_bitmask()` field list, so refine the checker instead of weakening the check globally.

4. `conversation-api-conventions.md` — commit `ef0e2e10977813dab4af8697fb70e4b7252df47e`
   - From John Thacker's merged !11676: do not repurpose `conversation_new()`'s options argument as a protocol discriminator; use an API/key that actually represents the intended identity. Persistent configuration and per-dissection scratch data must also use the lifetime scopes that own them, and persistent conversation data should not be recreated on redissection.

## Per-MR review ledger

| MR | State / weight | Review result |
|---|---|---|
| !11712 | Merged / medium | `sharkd` SIP statistics initialization fix. Makes `MIN_SETUP_COLUMN`, count/average user data, and reset behavior start from defined state; useful local evidence for init/reset symmetry, but no new notebook rule beyond existing state-initialization guidance. |
| !11711 | Merged / medium | `sharkd` frame output adds a distinct `fn` field for the header-field abbreviation while retaining the filter expression separately. Useful output-contract/API evolution evidence; tests were updated with the output schema. |
| !11710 | Merged / medium-high | João Valverde removes the obsolete Funnel logger callback after the wslog API superseded it. Good architectural cleanup: remove redundant wrapper interfaces once a shared logging facility is the real owner. No new rule promoted because shared-helper/API-deprecation guidance already covers it. |
| !11709 | Merged / medium | John Thacker fixes Qt5/clang compilation where `QByteArray` takes `int` length in Qt5 but `qsizetype` in Qt6. Reinforces compiling against the supported version-specific API signature instead of assuming Qt5/Qt6 typedef equivalence. |
| !11708 | Merged / normal | Adds five IEEE 802.11 EXT element dissectors and AKM values. Feature/spec update; no cross-cutting convention extracted. |
| !11707 | Merged / normal | Adds DiffServ codepoint 44. Standards/value-table update; no durable cross-cutting rule beyond keeping registry values current. |
| !11706 | Merged / normal | GitHub Windows-build maintenance replacing a removed/deprecated package-manager command. Build-infrastructure compatibility update; no new convention. |
| !11705 | Merged / normal | Companion/backport of the Windows GitHub build fix. Corroborative only. |
| !11704 | Merged / low | Spelling cleanup. No durable convention. |
| !11703 | Merged / **very high** | Guy Harris centralizes timestamp precision into `ws_tsprec_e`, whose fixed-precision values equal the number of decimal fractional digits, and aligns Wiretap/display precision constants to that semantic model. Strong foundational evidence for the notebook's existing timestamp-precision conventions; not duplicated into a second rule. |
| !11702 | Merged / medium-high | Qt Wayland fix: do not call `winId()` on a child widget when a top-level native window is required. Accepted platform-UI behavior; useful Qt portability evidence. |
| !11701 | Merged / medium | Companion/backport of the Qt Wayland `winId()` fix. Corroborative only. |
| !11700 | Merged / high | HCI USB classification changes from sample endpoint addresses to specification-required USB transfer types. Maintainer review also retained fallback behavior for `URB_UNKNOWN`. Durable protocol lesson: classify by normative semantics, not example numbering; existing protocol-version/implementation guidance is sufficient. |
| !11699 | Merged / normal | Adds `voip-calls` and `voip-convs` taps to sharkd. Feature/API exposure; no broader convention extracted. |
| !11698 | Merged / **high** | Martin Mathieson refines bitmask checking so an all-set mask is accepted only for the first/sole `hf_` in the bitmask list. Promoted to `typed-item-checker-conventions.md`. |
| !11697 | Merged / normal | BLF file-dissection improvements. Useful format-specific maintenance, but no new cross-cutting rule promoted. |
| !11696 | Merged / **high** | Guy Harris fixes BLF formatting: unsigned quantities use `%u`, and unknown encapsulations are rendered in the decimal namespace in which `WTAP_ENCAP_*` values are defined. Strong type/namespace fidelity evidence; existing C-format/type rules cover the general lesson. |
| !11695 | Merged / normal | DNP `val_to_str_ext()` fallback-format correction. Reinforces supplying correct fallback formatting for value-string helpers. |
| !11694 | Merged / high | João Valverde replaces the GUI-exposed Lua `console.lua` implementation with a native Qt Lua console, using the embedded Lua engine while moving GUI design to the layer with full Qt APIs. Useful ownership/UI architecture evidence; no general rule promoted here. |
| !11693 | Merged / normal | Bluetooth HCI v5.4 event-mask update. Specification feature update. |
| !11692 | Merged / medium | IPERF field/filter-name cleanup, including consecutive filter names and unused include removal. Corroborates static checker/field-registration hygiene. |
| !11691 | Merged / normal | Companion Bluetooth HCI v5.4 event-mask change. Corroborative feature update. |
| !11690 | Merged / **high** | John Thacker fixes malformed BT-DHT errors where a child bencoded decoder returns 0 on failure; propagating that failure is required so callers do not treat a non-advancing offset as success and enter recursion/infinite loops. Strong corroboration for existing parser-progress/consumption-boundary rules. |
| !11689 | Merged / **high** | Martin Mathieson enables `--check-bitmask-fields` in the normal GitLab CI typed-item checker invocation. Promoted to `typed-item-checker-conventions.md`. |
| !11688 | Merged / medium | DO-IRP cleanup: static tables/includes and fixed mask widths. Corroborates field-mask and translation-table hygiene. |
| !11687 | Merged / medium-high | DHCP relay-agent sub-options may legally have zero length per RFC 3046; removes an incorrect ERROR diagnostic. Reinforces that expert info should diagnose actual protocol violations, not assumptions stricter than the specification. |
| !11686 | Merged / normal | Adds Cassandra/CQL EVENT message dissection. Protocol feature; no general convention extracted. |
| !11685 | Merged / medium | Updates multiple IEEE 802.11 EHT elements to draft 3.0 and includes unit-test work. Standards evolution evidence, already covered by protocol-version compatibility guidance. |
| !11684 | Merged / medium | Enables the browser `SSLKEYLOGFILE` Lua launcher by default after the preceding disabled-code/revert discussion. Accepted resolution of the !11679 debate; no standalone policy generalized. |
| !11683 | Merged / low | Automatic data update for 2023-08-13. Generated/reference-data maintenance. |
| !11682 | Merged / low | Automatic data update companion branch. No new convention. |
| !11681 | Merged / low | Automatic data update companion branch. No new convention. |
| !11680 | Merged / medium | F1AP off-by-one constant fix. Format-specific correctness fix; no broader rule beyond exact spec bounds. |
| !11679 | **Closed/unmerged / low** | Proposed revert of the disabled browser SSLKEYLOG launcher. Discussion considered documentation vs enabling the feature; later merged !11684 enabled it instead. Down-weighted as superseded design discussion. |
| !11678 | Merged / low-medium | Removes never-enabled `dtd_gen.lua`. Useful dead-code cleanup evidence; no new rule. |
| !11677 | Merged / medium | Stable-branch Qt Wayland `winId()` fix. Corroborates !11702. |
| !11676 | Merged / **high** | John Thacker fixes IDN conversation API misuse, persistent configuration allocation, and packet-scope message allocation. Promoted to `conversation-api-conventions.md`. |
| !11675 | Merged / medium | Moves autoscroll's changing runtime state into recent-state handling rather than treating it solely as a static preference. Useful UI-state ownership evidence; no new general rule promoted. |
| !11674 | Merged / low | Removes an obsolete `<sys/wait.h>` include after the code stopped using wait-status macros. Straightforward dependency cleanup. |
| !11673 | Merged stable backport / high corroboration | Guy Harris release-4.0 backport of DLT-to-LINKTYPE normalization. Promoted together with master !11672. |
| !11672 | Merged master / **extremely high** | Guy Harris explains why host `DLT_*` and portable `LINKTYPE_*` values cannot be assumed numerically identical and normalizes dumpcap boundaries. Promoted to `capture-linktype-conventions.md`. |
| !11671 | Merged stable backport / high corroboration | Backport of the pcap timestamp-struct portability fix. Promoted with master !11665. |
| !11670 | Merged / medium | Ensures autoscroll is actually disabled when the preference says it is off, rather than merely setting the initial action state. Corroborates keeping UI action state and effective behavior synchronized. |
| !11669 | Merged / medium | Restores Qt autoscroll behavior to the `toggled` signal semantics. UI signal/state correctness; no broader convention extracted. |
| !11668 | Merged stable backport / high corroboration | Guy Harris backport guarding `RLIMIT_AS` by actual availability and reporting `setrlimit()` errors. Promoted with master !11664. |
| !11667 | Merged stable/backport / medium-high | Adds `<sys/ioctl.h>` because it is the declaring header for `ioctl()` on the relevant BSD path. Reinforces including the header that owns a declaration instead of depending on transitive declarations. |
| !11666 | Merged / medium | Companion autoscroll `toggled` behavior fix. Corroborative UI-state work. |
| !11665 | Merged master / **extremely high** | Guy Harris avoids assigning `struct timeval` wholesale to `pcap_pkthdr.ts` because libpcap does not guarantee exact type identity; copies members instead. Promoted to `platform-api-portability-conventions.md`. |
| !11664 | Merged master / **extremely high** | Guy Harris guards `RLIMIT_AS` on actual availability because OpenBSD 7.3 lacked it despite standards provenance, and reports the native setrlimit error. Promoted to `platform-api-portability-conventions.md`. |
| !11663 | Merged master / high | Adds `<sys/ioctl.h>` because `ioctl()` is used for BSD interface descriptions. Strong header-ownership/portability evidence; retained as corroboration rather than another notebook rule. |

## Frontier

`!11662` (`Qt: Redraw the packet bytes when our screen changes.`) exists in the same corpus commit and is merged. It was inspected only to establish the next descending frontier and **was not reviewed or counted in this run**.

The corpus therefore has not run out; no scraper restart is required.