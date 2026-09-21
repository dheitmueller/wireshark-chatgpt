# Wireshark MR automation review: !14660–!14709

- Reviewed on: 2026-09-21
- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `1c7ad509887ee25079a7865cc62ba18cba06f49a`
- Notebook starting commit: `c76f9d403f3558edab5be1bba220346573956a7c`
- Reviewed in this run: **50 MRs**
- Outcome mix: **50 merged**

## Selection and duplicate avoidance

Before selecting this batch, the review state was reconstructed from the available tracking in `dheitmueller/wireshark-chatgpt`: `reviewed-mrs.md`, the `reviewed-mrs-automation/` ledger inventory, and in particular the immediately preceding exact ledger `reviewed-mrs-automation-14710-14748-plus-26567-26577.md`. The historical !17571–!17620 batch remains part of the reviewed set and was preserved/counting during selection.

The corpus commit is unchanged from the preceding run. That run reviewed the then-new high MRs !26567–!26577 and the descending frontier through !14710, and verified that !26578 and !26579 were absent from this corpus snapshot. Exact reviewed-set subtraction therefore yields the next fifty highest-numbered unreviewed corpus MRs as **!14709 through !14660 inclusive**. No numeric range was inferred as reviewed merely from a partial ledger.

Exact MRs reviewed in this run:

`!14709, !14708, !14707, !14706, !14705, !14704, !14703, !14702, !14701, !14700, !14699, !14698, !14697, !14696, !14695, !14694, !14693, !14692, !14691, !14690, !14689, !14688, !14687, !14686, !14685, !14684, !14683, !14682, !14681, !14680, !14679, !14678, !14677, !14676, !14675, !14674, !14673, !14672, !14671, !14670, !14669, !14668, !14667, !14666, !14665, !14664, !14663, !14662, !14661, !14660`

## Per-MR review ledger

| MR | Outcome / depth | Review finding |
| --- | --- | --- |
| !14709 | Merged / deep | John Thacker extends `tshark -e` to compile/evaluate normal display-filter expressions rather than inventing a second expression language; `-E` output behavior is retained. Strong shared-semantic-engine design evidence, but no separate notebook rule was needed. |
| !14708 | Merged / deep, very high authority | Guy Harris authored and merged the CLX000 text wiretap reader. Important base implementation context; the already-reviewed follow-up !14713/!14714 hardening changes provide the stronger final precedent for checked numeric parsing and `WTAP_ERR_BAD_FILE`, so this MR is weighted highly but not used to supersede those later fixes. |
| !14707 | Merged / deep | John Thacker fixes RTPS malformed-input exits that returned zero from a helper whose contract is an end offset. Returning `offset + size` preserves monotonic caller state and avoids negative item lengths. Promoted to `parser-return-contract-conventions.md`. |
| !14706 | Merged / scanned | Broad spelling/highlighting maintenance. No durable architecture or submission rule beyond existing tooling hygiene. |
| !14705 | Merged / deep | John Thacker fixes tap-dialog actions created with no valid capture file. Initial widget state must be derived from current backing-object validity, not only later close events. Added to `ui-lifecycle-callback-conventions.md`. |
| !14704 | Merged / deep | John Thacker preserves dissector-table initial/default handles while clearing mutable auto-port/Decode-As bindings; entries are deleted only when no initial registration exists. Promoted to `dissector-table-binding-conventions.md`. |
| !14703 | Merged / scanned | Makes ASN dissector variables translation-unit-local; corroborates the existing internal-linkage convention for file-private state. |
| !14702 | Merged / scanned | Lua test compatibility: converts time to an integer where older Lua `os.date` rejects fractional seconds. Useful cross-version test maintenance, no new general rule. |
| !14701 | Merged / scanned | Lua 5.4 test compatibility adjustments. Compatibility maintenance; no additional cross-cutting convention. |
| !14700 | Merged / scanned | Lua 5.4 build compatibility. Useful portability change but no new notebook rule. |
| !14699 | Merged / scanned | Updates bundled bit-operation compatibility code for Lua 5.3/5.4. Version-portability maintenance. |
| !14698 | Merged / scanned | Adds OpenCORE AMR-NB development dependency to Debian setup tooling. Build-environment maintenance only. |
| !14697 | Merged / corroborating | Cherry-picked counterpart of the ICMPv6 zero-length recursion-path correction. Corroborates existing bounded-recursion/guard-placement guidance. |
| !14696 | Merged / corroborating | Same ICMPv6 zero-length recursion-path correction on another supported branch; no independent lesson beyond the primary fix. |
| !14695 | Merged / corroborating | Same accepted ICMPv6 recursion correction carried to a release branch; no additional convention. |
| !14694 | Merged / focused | John Thacker fixes the AID custom-address string size and filtering representation. Strong corroboration of !14689: custom semantic address types must honor the address API's formatting/filtering contract. |
| !14693 | Merged / focused | Primary ICMPv6 zero-length-option recursion correction: the zero-length malformed path should not take a recursion check intended for actual recursive descent. Reinforces existing recursion rules. |
| !14692 | Merged / deep | John Thacker makes `http2_get_header_value()` return safely decoded US-ASCII/UTF-8 text rather than arbitrary raw bytes, while documenting higher-level caller decoding and a possible separate raw API. Promoted to `protocol-text-api-conventions.md`. |
| !14691 | Merged / scanned | Enables libsmi configuration-file loading according to the library contract. Integration/configuration behavior, no broader rule extracted. |
| !14690 | Merged / scanned | Moves Windows Lua dependency to 5.4.6. Dependency/version maintenance only. |
| !14689 | Merged / deep | John Thacker replaces direct 802.11 RA/TA/AID column text with semantic `pinfo` address state and registered address types, fixing tunneled captures and generated filters. Promoted to `packet-address-semantics-conventions.md`. |
| !14688 | Merged / corroborating | Release counterpart of Gerald Combs's ASAP recursion/error-context update; reinforces explicit `in_error_pkt` context rather than generic repeated recursion. |
| !14687 | Merged / corroborating | Same ASAP recursion/error-context correction on another maintained branch; no independent rule. |
| !14686 | Merged / corroborating | Same ASAP recursion/error-context correction on another maintained branch; no independent rule. |
| !14685 | Merged / corroborating | Release counterpart adding recursion protection to Bencode/BT-DHT/ErlDP/H.233/H.264; part of the already well-represented bounded-recursion hardening series. |
| !14684 | Merged / corroborating | Same recursion-hardening series on another maintained branch; no new rule. |
| !14683 | Merged / corroborating | Same recursion-hardening series on another maintained branch; no new rule. |
| !14682 | Merged / scanned | Lua 5.4 test compatibility counterpart. No new cross-cutting convention. |
| !14681 | Merged / focused | Primary Bencode/BT-DHT/ErlDP/H.233/H.264 recursion-hardening change. Reinforces explicit bounded-recursion handling already recorded from later, stronger review evidence. |
| !14680 | Merged / corroborating | BEEP/Kafka/OpenFlow recursion-hardening counterpart. Existing recursion guidance covers it. |
| !14679 | Merged / corroborating | BEEP/Kafka/OpenFlow recursion-hardening counterpart. No additional convention. |
| !14678 | Merged / corroborating | BEEP/Kafka/OpenFlow recursion-hardening counterpart. No additional convention. |
| !14677 | Merged / corroborating | Banana/ISAKMP/KINK/M2UA/M3UA recursion-hardening counterpart. Existing recursion guidance applies. |
| !14676 | Merged / corroborating | Same Banana/ISAKMP/KINK/M2UA/M3UA recursion-hardening series on another branch. |
| !14675 | Merged / corroborating | Same Banana/ISAKMP/KINK/M2UA/M3UA recursion-hardening series on another branch. |
| !14674 | Merged / focused | Primary BEEP/Kafka/OpenFlow recursion checks/suppressions. Accepted evidence for context-aware recursion control, but later reviewed material already supplies stronger conventions. |
| !14673 | Merged / focused | Primary Banana/ISAKMP/KINK/M2UA/M3UA recursion checks. Strong historical corroboration of bounded recursive dissection, no duplicate notebook rule added. |
| !14672 | Merged / corroborating | Release counterpart removing redundant ICMPv6/NETLINK recursion checks where packet consumption already bounds recursion. Reinforces that recursion guards should model actual unbounded paths rather than be added mechanically. |
| !14671 | Merged / corroborating | Same removal of redundant recursion checks on another maintained branch; no independent rule. |
| !14670 | Merged / corroborating | Same removal of redundant recursion checks on another maintained branch; no independent rule. |
| !14669 | Merged / scanned | Adds FGCP Session Protocol payload dispatch to IPv4. Protocol-specific feature; no durable cross-cutting lesson identified. |
| !14668 | Merged / deep | SMTP AUTH fix demonstrates that explicit protocol state must outrank a generic lexical command heuristic; contributor supplied a focused PCAP reproducer. Promoted to `protocol-state-dispatch-conventions.md`. |
| !14667 | Merged / scanned | Updates setup scripts to install/use Lua 5.3. Dependency-transition maintenance. |
| !14666 | Merged / focused | Replaces `lua_pushnumber` with `lua_pushinteger` where values are semantically integral. Corroborates existing semantic numeric-domain/type guidance. |
| !14665 | Merged / focused | Primary ICMPv6/NETLINK change removes recursion checks where recursive progress is inherently bounded by packet consumption. Useful negative evidence against indiscriminate recursion guards; existing recursion notes already cover the principle. |
| !14664 | Merged / deep, high authority | Gerald Combs authored and merged the primary ASAP update that marks error-cause dissection with `pinfo->flags.in_error_pkt` and suppresses normal recursion handling in that context. Strong accepted recursion/context evidence, retained as corroboration because the notebook already has extensive recursion guidance. |
| !14663 | Merged / scanned | Lua test-time integer conversion counterpart. Compatibility maintenance only. |
| !14662 | Merged / scanned | Lua tests use `load` instead of removed `loadstring` on post-5.1 Lua. Version-compatible test maintenance. |
| !14661 | Merged / scanned | Stops testing Lua functions removed by newer Lua releases. Version-aware test maintenance. |
| !14660 | Merged / scanned | O-RAN FH CUS comments/string-text cleanup. No durable coding or architecture lesson beyond ordinary maintenance quality. |

## Durable notebook updates from this run

- `parser-return-contract-conventions.md`: cursor/end-offset helpers must preserve their positional contract on malformed-input exits; an error sentinel must not move a caller backward when the API promises an end position. Evidence: !14707.
- `ui-lifecycle-callback-conventions.md`: initialize action enablement from current backing-resource validity as well as lifecycle notifications. Evidence: !14705.
- `dissector-table-binding-conventions.md`: keep default/initial dissector registrations separate from mutable current overrides; resetting an override must not destroy the default binding. Evidence: !14704.
- `protocol-text-api-conventions.md`: text-returning protocol APIs should establish a safe documented encoding boundary and keep raw-octet access separate. Evidence: !14692.
- `packet-address-semantics-conventions.md`: populate semantic packet address state rather than writing only column text, so late column generation, tunneling, resolution, and filter generation stay coherent. Evidence: !14689, corroborated by !14694.
- `protocol-state-dispatch-conventions.md`: explicit protocol state outranks a broad lexical heuristic once the state machine has already determined the payload role. Evidence: !14668.

High-authority evidence was weighted accordingly. In particular, Guy Harris's authored-and-merged !14708 was treated as strong base-reader evidence, while the later already-reviewed !14713/!14714 corrections remain the stronger final parsing precedent; Gerald Combs's authored-and-merged !14664 was similarly treated as strong corroboration for recursion/error-context handling.

## Continuation

The corpus is not exhausted. `mr_14659.json` exists at the same corpus commit and is merged, so absent a future corpus update that introduces higher-numbered unreviewed MRs, the next descending candidate is !14659.
