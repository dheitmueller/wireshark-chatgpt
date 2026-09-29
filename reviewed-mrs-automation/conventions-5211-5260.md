# Durable conventions from Wireshark MRs 5211-5260

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly !5260 through !5211. Merged master changes are primary evidence; stable branches corroborate them. Closed !5224 is lower-weight history only.

## API shape
Merged !5260 splits `format_size()` into a mutually exclusive unit enum and separate prefix flags. Model independent semantic dimensions independently: use one choice for the mode and flags only for orthogonal modifiers.

## Multi-PDU datagrams
Merged master !5235, with !5248 and !5249 backports, iterates over multiple Foundation Fieldbus PDUs in one UDP payload. Validate each declared PDU length, create a bounded subset tvbuff, and advance by the inner dissector's actual consumed length. A datagram boundary is not necessarily an application-PDU boundary.

## Parser termination
Merged !5225, with !5226 and !5230 backports, adds a conservative RTMPT stop condition where wrapped sequence numbers defeat the traversal's monotonic assumptions. Guaranteed termination can take precedence over perfect recovery in a rare ambiguous edge case; document the tradeoff.

## Time parsing
Merged !5219 and !5222 validate the ISO-8601 prefix before probing fixed separator offsets and carry the selected Basic/Extended syntax mode forward. Merged !5231/!5232 move timezone arithmetic out of broken-down hour/minute fields into epoch seconds. Later !5668 remains authoritative for offset sign direction, so retain the normalized-arithmetic lesson but not the older sign behavior.

## Lifecycle supersession
Merged !5227 centralized Wiretap init/cleanup under epan, but later merged !5683 reverted that architecture after an exit crash. Lifecycle ownership must be validated through shutdown and ordering, and the later revert is authoritative.

## Registration side effects
Merged !5218 removes an unnecessary WebSocket preference-change handoff callback because rerunning it re-registered the dissector in the TCP table. Install a preference apply callback only when preference changes actually require rebinding or recomputation.

## Documentation maintenance
Merged !5217 contains Jaap Keuter and Gerald Combs guidance to use bare Doxygen `@file` rather than embedding filenames that can go stale after renames. Broad documentation cleanups should follow an agreed policy the project is willing to maintain.

## Optional-feature builds
Merged !5243 fixes HTTP/2 declarations/callbacks that escaped nghttp2 guards and failed when the optional library was absent. This is early corroboration for the later No-Options CI rule: dependency-off builds are a real supported configuration.

## Review scope
In merged !5245, Jorg Mayer explicitly reported compile/run testing with Qt 5.12.10 and 6.2.1 on macOS while declining to claim semantic C++ review. Record exactly what a validation pass establishes; cross-platform smoke testing and code-semantic review are different evidence.
