# Review findings — Wireshark MRs !1860–!1909

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed the exact fifty-MR set recorded in `ledger-1860-1909.md`. Merged work is treated as stronger implementation precedent than closed or superseded work; authoritative maintainer review is preserved even when the proposed implementation did not merge.

## Strong durable findings

- **!1895 — merged, Guy Harris authored/merged.** Replaces a generic Wiretap structured-option blob for pcapng `if_filter` with a discriminated semantic type and dedicated accessors, improving compile-time type checking. The writer also checks whether the selected variant fits the 16-bit pcapng option-length field instead of masking a too-large length. Promoted to `wiretap-option-type-conventions.md`.
- **!1900 — merged, John Thacker; João Valverde review.** A generic CLI preference parser must not reject empty values before knowing the preference type. Empty is valid for string/range preferences and can be required to override a persisted non-empty value; “explicit empty” is separate from “reset to default.” Promoted to `empty-value-api-conventions.md`.
- **!1879 — merged, Guy Harris authored/merged.** Moves built-in and plugin tap-listener registration behind one libwireshark API so each frontend does not duplicate the library's internal registration order/policy. Promoted to `registration-extension-point-conventions.md`.
- **!1876 → !1893 — closed predecessor plus merged successor.** Pascal Quantin rejected making `get_udp_conversation_data()` private solely because there were no current in-tree callers; exported helpers can be useful to plugins and API parity matters. The merged successor keeps the helper public. Promoted to `abi-compatibility-conventions.md`.
- **!1889 — merged, Anders Broman and Jaap Keuter review.** ZVT exposed that existing BCD helpers encoded one nibble-order convention rather than generic BCD semantics. Review favored extending the shared helper with an explicit digit-order variant rather than adding a local one-off decoder; the contributor also supplied a capture and updated the public symbol manifest. This corroborates existing bit-order and public-symbol rules.
- **!1864 — merged, substantial Graham Bloice/Peter Wu review.** DNP3-over-TLS should reuse the TCP dissector entry point, and a reusable named handle should come from `register_dissector()`. Testing with a supplied capture plus TLS decryption data exposed a nested reassembly case where frame number alone did not identify the valid reassembly context; current layer identity also mattered. This corroborates registration and layered-reassembly conventions already present.
- **!1903 — merged, João Valverde review.** Adding an entry out of sort order to a `value_string_ext` table triggered a warning and forced linear search. The accepted fix restored ordering.
- **!1892 — merged.** SOME/IP UAT post-update callbacks rebuild not only primary configuration maps but also dynamic `hf_` registrations derived from them. This independently corroborates the existing UAT rule that `post_update_cb` must refresh every derived representation after accepted table changes.
- **!1885 — merged.** Martin Mathieson used `./tools/check_typed_item_calls.py --commits 1` to catch an `FT_UINT8` field being added with length 3. This is direct historical corroboration of the notebook's typed-item checker guidance.
- **!1863 — merged, João Valverde with Jaap Keuter review.** Generated/private `config.h` must not leak into installed/system-facing headers. This corroborates the public-header rule already captured from later work.
- **!1873–!1869 — merged statistics series.** MTP3, SIP, RPC, DHCP, and ANSI-A statistics tables use `stat_tap_find_table()` to create fixed table structure only once. This is the same lifecycle pattern already represented by the later, larger statistics-table series, so it is recorded here without duplicating the convention.

## Per-MR accounting

| MR | State | Review disposition |
| --- | --- | --- |
| !1909 | merged | Plugin documentation corrected to the actual four required symbols; no new rule beyond keeping developer docs synchronized with the plugin ABI. |
| !1908 | merged | Gerald Combs' initial CONTRIBUTING file; reproducer/capture guidance corroborates existing submission/testing conventions. |
| !1907 | merged | Static-symbol cleanup; review notes `tools/check_tfs.py --common` for deciding when a true/false string is genuinely common. |
| !1906 | merged | IPv6 CRH32 generated field used the CRH16 `hf_`; narrow correctness fix. |
| !1905 | merged | User-guide caption cleanup only. |
| !1904 | merged | Removes a misleading fixed array bound from a function parameter; corroborates C array-parameter semantics. |
| !1903 | merged | IPv6 TPF addition; sorted `value_string_ext` invariant was caught during pipeline testing. |
| !1902 | merged | Qt byte-view width calculations use one helper consistently to avoid clipping across Qt metric APIs. |
| !1901 | merged | SCTP documentation; discussion incidentally exposed an unrelated string truncation issue, not part of the accepted change. |
| !1900 | merged | Empty CLI preference values preserved for semantic type handling; promoted. |
| !1899 | closed | Tiny TFTP comment correction; Anders Broman requested commit cleanup/squashing. Low-weight workflow evidence only. |
| !1898 | merged | Broad internal-symbol static cleanup; no distinct convention. |
| !1897 | merged | NTP refid presentation no longer truncates escaped/non-ASCII representation to source-byte count; corroborates text-encoding/display-length rules. |
| !1896 | merged | D-Bus request/reply state enriches sparse responses and uses first-pass conversation/transaction storage plus generated response metadata; corroborates request-response state conventions. |
| !1895 | merged | Guy Harris Wiretap typed `if_filter` option redesign and serialized-length checks; promoted. |
| !1894 | closed | Ruckus dictionary update blocked by source-branch/collaboration setup; corroborates focused topic-branch guidance, but implementation is unmerged. |
| !1893 | merged | Accepted successor to !1876's symbol cleanup while retaining the disputed public UDP conversation helper. |
| !1892 | merged | SOME/IP UAT post-update now refreshes dynamic field registrations as well as data maps; corroborates UAT derived-state lifecycle. |
| !1891 | merged | RTMP type-3 extended-timestamp fix validated against a reproducing capture; explicitly incomplete for a rarer case, so not generalized. |
| !1890 | merged | 802.11ax extension updates; protocol-specific. |
| !1889 | merged | ZVT BCD order review led to shared helper extension, capture validation, and symbol-manifest update. |
| !1888 | merged | Gerald Combs makes RTPproxy explicitly distinguish IPv4/IPv6 rather than treating every non-IPv4 address as IPv6; corroborates address-domain validation. |
| !1887 | merged | SMPP documentation only. |
| !1886 | closed | Guy Harris predecessor of merged !1895; superseded and therefore down-weighted. |
| !1885 | merged | Robust AV Streaming feature; typed-item checker caught field-width misuse; existing checker rule corroborated. |
| !1884 | merged | Developer-guide packaging/CI documentation update. |
| !1883 | merged | 802.11 Extended Capabilities expansion, tested with WFA captures; protocol feature. |
| !1882 | merged | Automatic release-3.2 data/translation update; no new engineering convention. |
| !1881 | merged | Automatic release-3.4 data/translation update; no new engineering convention. |
| !1880 | merged | Automatic master data/translation update; no new engineering convention. |
| !1879 | merged | Guy Harris centralizes tap-listener registration in libwireshark; promoted. |
| !1878 | merged | Guy Harris restores an accidentally deleted comment line; trivial. |
| !1877 | merged | Guy Harris generates the tap plugin registration source with the standard generator rather than checking it in; corroborates generated-registry conventions. |
| !1876 | closed | Public-symbol cleanup predecessor; Pascal Quantin's plugin-API objection retained, implementation superseded by !1893. |
| !1875 | merged | Guy Harris indentation-only cleanup. |
| !1874 | merged | John Thacker removes EOL openSUSE 15.1 CI packaging target; routine build-matrix maintenance. |
| !1873 | merged | MTP3 statistics table created only once; corroborates existing lifecycle rule. |
| !1872 | merged | SIP statistics tables created only once; corroborating. |
| !1871 | merged | RPC statistics table created only once; corroborating. |
| !1870 | merged | DHCP statistics table created only once; corroborating. |
| !1869 | merged | ANSI-A DTAP statistics table created only once; corroborating. |
| !1868 | merged | Guy Harris completes the Wiretap nth-string formatted setter API family and exports it; narrow API completeness. |
| !1867 | merged | Guy Harris comment cleanup only. |
| !1866 | merged | Guy Harris renames generic option terminology from “custom” to “structured” to avoid collision with pcapng Custom Options; later superseded architecturally by !1895's stronger typed API. |
| !1865 | merged | Gerald Combs changes documentation macro wording from Bug to Issue; docs-only. |
| !1864 | merged | DNP3/TLS integration, named registration handles, capture validation, and layer-aware reassembly behavior; strong corroborating evidence. |
| !1863 | merged | Installed/system-facing headers must not depend on private generated `config.h`; corroborates existing public-header guidance. |
| !1862 | merged | Modbus/TCP Security over TLS; Peter Wu notes TLS registration already supplies Decode As and questions unnecessary port preferences/heuristics. Useful dispatch context, no new rule promoted. |
| !1861 | merged | TCP SACK/BiF analysis now checks optional analysis state before dereference; narrow state-guard correctness fix. |
| !1860 | merged | Version bump only. |

No SMPTE ST 291/VANC packet type was encountered.
