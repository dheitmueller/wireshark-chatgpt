# Wireshark MR review automation ledger: !14510–!14559

Corpus commit reviewed: `1c7ad509887ee25079a7865cc62ba18cba06f49a`

Selection method: rebuilt the already-reviewed MR set from the available `reviewed-mrs.md`, aggregate automation tracking, and per-run ledgers in `reviewed-mrs-automation/`, explicitly including the historical !17571–!17620 batch and the mixed !26567–!26577 / !14710–!14748 ledger. `mr_26578.json` is absent at this corpus commit. The fifty highest-numbered corpus MRs not already present in that reviewed set are exactly !14559 through !14510. No numeric range was assumed reviewed merely from partial ledger coverage.

Outcome: all 50 selected MRs were merged. Merged changes are therefore weighted as accepted upstream evidence; repeated stable-branch recursion backports and compatibility-series changes are treated mainly as corroboration when later or stronger notebook guidance already exists.

| MR | Title | Review result |
|---|---|---|
| !14559 | ptp: Improve display of rateRatio allowing more decimals. | Merged. Display/precision cleanup; no new durable rule beyond existing field-display semantics. |
| !14558 | thrift: make some functions static | Merged. Reinforces internal-linkage hygiene for file-local helpers. |
| !14557 | Qt: Add case sensitive find to Follow Stream and Show Packet Bytes | Merged. UI feature extension; no broader convention promoted. |
| !14556 | WiFi(Nan): Update DCA, Availabilities, and CSIA to NAN R4 spec | Merged. Protocol-spec update; no cross-cutting convention promoted. |
| !14555 | SCCP: Use register_dissector_preference | Merged, authored and merged by John Thacker. Promoted: use the semantically typed preference registration API when a preference names a framework object such as a dissector rather than treating it as arbitrary text. |
| !14554 | RNSAP: Dissect IMSI | Merged. Protocol coverage extension; no broader convention promoted. |
| !14553 | Inject TLS Secrets: Check for NULL used secrets map | Merged stable-branch backport. Reinforces guarding nested optional state before iteration/dereference. |
| !14552 | Inject TLS Secrets: Check for NULL used secrets map | Merged master fix, authored by John Thacker. Same nested-optional-state lesson; existing nullability guidance already covers it. |
| !14551 | Look into some items where VALS doesn't fit in mask | Merged, authored and merged by Martin Mathieson. Promoted: value-string keys for masked fields belong to the post-mask/post-shift logical value domain; checker warnings about values outside a mask usually signal a semantic mismatch. |
| !14550 | nmea0183: Add generic NMEA0183 dissector | Merged. Generic fallback dissection and checksum support added; Jaap Keuter explicitly asked whether it had been fuzz-tested, reinforcing fuzzing expectations for new parsers. |
| !14549 | falcodump: add missing AWS regions | Merged. Data/list maintenance; no durable convention promoted. |
| !14548 | ICMPv6: Add a recursion check | Merged recursion hardening. Corroborates existing bounded-recursion guidance. |
| !14547 | ICMPv6: Add a recursion check | Merged recursion hardening/backport. Corroboration only. |
| !14546 | ICMPv6: Add a recursion check | Merged recursion hardening/backport. Corroboration only. |
| !14545 | DHCPv6: Add a recursion check | Merged recursion hardening. Corroborates existing bounded-recursion guidance. |
| !14544 | SMB2: Add recursion checks | Merged recursion hardening. Corroborates existing bounded-recursion guidance. |
| !14543 | SMB2: Add recursion checks | Merged recursion hardening/backport. Corroboration only. |
| !14542 | DHCPv6: Add a recursion check | Merged recursion hardening/backport. Corroboration only. |
| !14541 | DHCPv6: Add a recursion check | Merged recursion hardening/backport. Corroboration only. |
| !14540 | SMB2: Add recursion checks | Merged recursion hardening/backport. Corroboration only. |
| !14539 | SMB2: Add recursion checks | Merged recursion hardening/backport. Corroboration only. |
| !14538 | ICMPv6: Add a recursion check | Merged recursion hardening/backport. Corroboration only. |
| !14537 | DHCPv6: Add a recursion check | Merged recursion hardening/backport. Corroboration only. |
| !14536 | extcap: Use extcap.cfg as extcap config file name | Merged master fix by John Thacker, approved/merged by Anders Broman. Avoids a configuration filename colliding with the conventional extcap program directory name. Useful naming/configuration evidence, but too specific to promote separately. |
| !14535 | Fix some spelling errors | Merged editorial cleanup; no convention promoted. |
| !14534 | ptp: Corrected data type for cumulativeScaledRateOffset. | Merged. Corrects signedness per protocol definition and adds a derived rate-ratio representation; reinforces matching field types to wire semantics. |
| !14533 | dfilter: Add functions to override field base | Merged, authored and merged by John Thacker. Adds explicit display-filter functions for decimal/hex presentation without changing underlying field semantics; no broader new rule needed. |
| !14532 | OSITP: Pass ED-TPDU to subdissectors | Merged, authored and merged by John Thacker. Extends payload heuristic dispatch consistently to ED-TPDUs with data fallback; corroborates framework dispatch/fallback practices. |
| !14531 | Qt: Work around QTBUG-122109 when applying packet list style sheet | Merged, authored and merged by John Thacker. Version-specific Qt behavioral workaround; existing dependency-version compatibility guidance covers it. |
| !14530 | macos-setup-brew: Use LUA 5.3 | Merged. Dependency-version update in the Lua compatibility series; corroboration only. |
| !14529 | TCPCL: Add preference to allow missing Contact Header | Merged; merged by John Thacker. John caught that `raw_offset` was a `gint` while `g_int64_hash` reads a 64-bit key, which can read beyond the backing object. Strong corroboration for the existing exact backing-storage/API-width rule. |
| !14528 | AllJoyn: Add recursion checks | Merged recursion hardening. Corroboration only. |
| !14527 | AllJoyn: Add recursion checks | Merged recursion hardening/backport. Corroboration only. |
| !14526 | AllJoyn: Add recursion checks | Merged recursion hardening/backport. Corroboration only. |
| !14525 | QUIC: Handle early 1-RTT data from server | Merged master fix, authored by John Thacker and merged by Alexis La Goutte. Promoted: when the current coalesced element must be validated against its predecessor, perform the predecessor lookup before publishing the current element into the same searchable state container. |
| !14524 | 6LoWPAN: Add recursion checks | Merged recursion hardening. Corroboration only. |
| !14523 | 6LoWPAN: Add recursion checks | Merged recursion hardening/backport. Corroboration only. |
| !14522 | 6LoWPAN: Add recursion checks | Merged recursion hardening/backport. Corroboration only. |
| !14521 | AllJoyn: Add recursion checks | Merged recursion hardening/backport. Corroboration only. |
| !14520 | 6LoWPAN: Add recursion checks | Merged recursion hardening/backport. Corroboration only. |
| !14519 | LUA: Pickup LUA 5.3 or 5.4 if available and use it. | Merged. Build-system dependency selection in the Lua 5.3/5.4 compatibility series; existing compatibility guidance applies. |
| !14518 | LUA: Make it compile with LUA 5.4 | Merged, authored/merged by Anders Broman. Uses version guards for Lua API changes (`luaL_optinteger` versus older helpers); corroborates supported-version compatibility discipline. |
| !14517 | TN5250: Add a recursion check | Merged recursion hardening. Corroboration only. |
| !14516 | TN5250: Add a recursion check | Merged recursion hardening/backport. Corroboration only. |
| !14515 | TN5250: Add a recursion check | Merged recursion hardening/backport. Corroboration only. |
| !14514 | LUA bitop: Convert our lua_bitop.c to work with LUA 5.3 and 5.4 | Merged, authored/merged by Anders Broman. Compatibility adaptation across supported Lua numeric/API definitions; corroborates dependency compatibility guidance. |
| !14513 | TN5250: Add a recursion check | Merged recursion hardening/backport. Corroboration only. |
| !14512 | MONGO: Add a recursion check | Merged recursion hardening. Corroboration only. |
| !14511 | MONGO: Add a recursion check | Merged recursion hardening/backport. Corroboration only. |
| !14510 | MONGO: Add a recursion check | Merged recursion hardening/backport. Corroboration only. |

## Durable notebook changes promoted from this batch

- `preference-architecture-conventions.md`: semantic identifiers should use the framework's typed preference API when one exists; !14555 is the accepted dissector-preference example.
- `field-value-semantics-conventions.md`: `VALS` keys attached to masked integer fields represent extracted logical values after mask/shift, not raw positioned bits; !14551 provides accepted concrete corrections.
- `protocol-context-state-conventions.md`: predecessor-dependent checks in a shared/coalesced state container must run before publishing the current element when insertion would change what "previous" lookup returns; !14525 provides the accepted QUIC example.

The recursion series, Lua compatibility series, TLS NULL-map fix, TCPCL hash-key width review, Qt workaround, and NMEA fuzzing question were retained as corroborating evidence rather than duplicating stronger existing notebook rules.

Next descending corpus candidate at this corpus commit: !14509 (`mr_14509.json` exists). The corpus is therefore not exhausted.