# Review findings — !2611–!2660

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged master changes and substantive maintainer review are weighted most heavily. Stable-branch backports are corroboration, and closed !2638 is down-weighted.

| MR | Review | Findings |
|---|---|---|
| !2660 | Deep / corroboration | John Thacker fixes QCustomPlot analyzer findings; Anders Broman asks whether the vendored-source fixes were reported upstream. Reinforces the existing vendor-upstreaming rule. |
| !2659 | Scanned | Documents TShark rpcap authentication option `-A`. Documentation-only. |
| !2658 | Scanned | Makes Windows CI creation of `C:\Development` idempotent. CI maintenance. |
| !2657 | Scanned / corroboration | Pascal Quantin updates NGAP to 3GPP v16.5.0, changing ASN.1, cnf/template sources and generated dissector output together. |
| !2656 | Scanned / corroboration | Pascal Quantin updates X2AP to v16.5.0 with authoritative ASN.1/config/template and generated C synchronized. |
| !2655 | Scanned / corroboration | Pascal Quantin updates S1AP to v16.5.0 with authoritative ASN.1/config/template and generated C synchronized. |
| !2654 | Scanned | JSON packet printing uses `%09u` for nanoseconds so sub-second timestamps retain required leading zeroes. |
| !2653 | Deep / transitional | Adds mutex protection around singleton telephony dialogs, but review reports a remaining crash and later !2689 provides stronger corrective evidence. Useful race/lifecycle history, not a final architecture exemplar. |
| !2652 | Scanned | Corrects BGP SAFI display-filter abbreviations that accidentally reused the AFI names. Filter identity is semantic API surface. |
| !2651 | Corroboration / very high authority | Guy Harris-authored release-3.2 backport clarifies pcapng SHB 1.2 compatibility semantics and the supported-version test. |
| !2650 | Corroboration / very high authority | Guy Harris-authored release-3.2 backport of Sysdig pcapng event-v2 support from !2631. |
| !2649 | Corroboration / very high authority | Guy Harris-authored release-3.4 backport of the pcapng version-semantics clarification. |
| !2648 | Corroboration / very high authority | Guy Harris-authored release-3.4 backport of Sysdig pcapng event-v2 support. |
| !2647 | Scanned | Adds VHT NDPA extended station-info dissection and supplies a representative pcap. Reinforces capture-backed protocol submissions. |
| !2646 | Deep / extremely high authority | Guy Harris-authored master clarification: pcapng 1.2 is treated as 1.0 because adding block types does not justify a minor-version bump; writers should remain canonical. |
| !2645 | Scanned | SCTP association-analysis UI layout/text cleanup; only minor style review. |
| !2644 | Scanned | Updates SMB2 dissector to a newer Microsoft specification revision. Protocol-specific. |
| !2643 | Scanned | Temporarily switches Windows CI back to Qt 5.15.1 while diagnosing runner instability. |
| !2642 | Scanned | PROFINET RSI parsing now dispatches blocks using PDU type/opnum context and validates the service-response layout. Protocol-specific parser correction. |
| !2641 | Deep / transitional | RTP Player avoids calling audio-object actions from a Qt state callback that holds an internal mutex and uses delayed cleanup. Useful event-callback reentrancy evidence; later RTP work remains stronger. |
| !2640 | Deep / high authority | Anders Broman asks that TIFF internal data/function names carry the protocol prefix, including `dissect_tiff_*`, even though they are static. The author updates the code before merge. |
| !2639 | Scanned / qualified | MiBeacon dissector merged, but the contributor then stated the contribution was unauthorized and requested removal; later !2700 removes it. Do not use this as durable implementation precedent. |
| !2638 | Closed / down-weighted | Anders Broman asks for rebase/squash and less unrelated formatting; Lars Völker notes that unrelated/already-existing protocol changes are mixed together and asks for example files. Closed after useful pieces landed elsewhere. |
| !2637 | Scanned | Clears Windows CI dependency ordering. CI maintenance. |
| !2636 | Scanned | WSUG print-dialog documentation update; discussion mainly diagnoses unrelated Windows runner failures. |
| !2635 | Scanned | Adds PTP G.8275.2 profile fields; review is mainly formatting/style. |
| !2634 | Scanned / corroboration | John Thacker fixes warnings/deprecations in bundled QCustomPlot. Reinforces careful maintenance of vendored source. |
| !2633 | Deep / transitional | Large RTP Analysis/Player redesign supports more streams and singleton UI state; extensive user testing exposed follow-up crash/audio issues later corrected by other MRs. |
| !2632 | Scanned | Anders Broman adds PFCP QUOAF bit dissection. Narrow protocol update. |
| !2631 | Deep / extremely high authority | Extensive Guy Harris review of Sysdig pcapng support establishes version-compatibility semantics, explicit v1/v2 layout handling, and minimum-record-size validation before parsing. Merged after iterative corrections. |
| !2630 | Deep / correction | Completes the RTPS locator-port signedness repair by changing the remaining local variable and formatting to unsigned. |
| !2629 | Deep / high authority | Martin Mathieson fixes a remaining `proto_tree_add_item_ret_int()` call against an `FT_UINT32` field after `check_typed_item_calls.py` caught it. |
| !2628 | Scanned | Adds a Visual Studio code-analysis CI step. Tooling expansion. |
| !2627 | Scanned | RTP Player UI enable/disable and signal-wiring improvements. |
| !2626 | Deep / high authority | John Thacker fixes FT_PROTOCOL literal comparisons by giving valid literal values a non-NULL empty protocol string while preserving fully NULL newly allocated values as a distinct uninitialized/null-equivalence state. |
| !2625 | Deep / high authority | Pascal Quantin traces RTPS port semantics to the spec and requires unsigned field/type handling. The accepted change aligns hf type, returned C type, tree helper, and formatting; later checker-driven !2629/!2630 finish all call sites. |
| !2624 | Scanned | Switches the stable-3.2 Windows CI branch to the new runner. |
| !2623 | Scanned | Switches release-3.4 Windows CI to the new runner. |
| !2622 | Scanned / high authority | Guy Harris removes unused internal preference-effect flags. Dead internal API cleanup. |
| !2621 | Scanned | Reduces MSBuild CI verbosity. |
| !2620 | Scanned | Removes duplicate STUN code. |
| !2619 | Scanned / high authority | Guy Harris removes an unused public preference define that no code tests. |
| !2618 | Scanned | Switches master Windows CI to the new runner. |
| !2617 | Corroboration / very high authority | Guy Harris-authored stable-3.2 backport of optional synchronous MaxMind lookups. |
| !2616 | Deep | Replaces an endless/QWaitCondition capture-filter worker with a QObject moved to a QThread and queued signal/slot work; destructor explicitly quits and waits for the thread before destroying worker/thread objects. |
| !2615 | Discussion-focused | Removes unreachable TURN child parsing; Alexis La Goutte asks for the semantic reason, and the author explains that the parent has already consumed the relevant header. |
| !2614 | Deep / qualified | Leak cleanup changes shutdown ownership/order so Lua funnel cleanup can still use UI operations, but later discussion reports a shutdown crash in another path. Useful evidence that teardown ordering is a dependency graph, not a simple “delete everything earlier” optimization. |
| !2613 | Deep / high authority | Martin Mathieson's cppcheck cleanup is reviewed by Pascal Quantin, who requires semantically necessary NAS-5GS operations to remain while allowing truly redundant initializers to be removed. Static-analysis cleanup must preserve protocol state. |
| !2612 | Corroboration / very high authority | Guy Harris-authored release-3.4 backport of optional synchronous MaxMind lookups. |
| !2611 | Scanned | Represents IEEE 802.11 ISTA availability bits in a form usable by display-filter string searches. Protocol-specific presentation/filter enhancement. |

No SMPTE ST 291/VANC packet type was encountered in this batch.
