# Review findings — !2661–!2710

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are treated as accepted evidence. Closed !2704 and !2676 are retained only as lower-weight historical context.

| MR | Review | Findings |
|---|---|---|
| !2710 | Deep | Guy Harris moves Windows console creation/destruction outside the per-interface capability loop. The resource now spans the whole reporting operation, while error paths still destroy it before leaving. |
| !2709 | Scanned | WSDG documentation adds a pointer to the plugin interface demo; documentation-only. |
| !2708 | Scanned | Fixes the documented CMake option spelling for pluginifdemo; documentation-only. |
| !2707 | Deep | QCustomPlot performance fix changes vendored third-party code; Anders Broman explicitly asks whether it was also pushed upstream. Local vendor patches should be upstreamed when practical to reduce divergence. |
| !2706 | Deep | New SparkplugB dissector includes a sample capture and bundled .proto file. Review verifies an installed macOS package on another system, not only build-tree execution; cross-platform build caught a typo. New packaged runtime data should be tested from installed artifacts. |
| !2705 | Scanned | Corrects IEEE 802.11 trigger RU-allocation region handling before applying the custom formatter. |
| !2704 | Closed / down-weighted | Closed empty predecessor for Trigger Ranging display work; no accepted diff. Merged !2703 carries the implementation. |
| !2703 | Scanned | Reuses custom display functions for Trigger Ranging user-info fields. |
| !2702 | Scanned | Enables MSVC caret diagnostics to align compiler diagnostics with GCC/Clang. |
| !2701 | Scanned | Conversation-table start time honors epoch-based timestamp mode instead of always using capture-relative time. |
| !2700 | Scanned | Removes previously added MiBeacon support after the contributor states it was not authorized for contribution; provenance/governance cleanup rather than a coding convention. |
| !2699 | Deep | Guy Harris rejects silencing a documentation warning by deleting the semantic directive; he asks to keep the line and correct the mistaken parameter name instead. Fix the documentation contract rather than suppressing the checker. |
| !2698 | Deep | Guy Harris centralizes interface-capability validation and exit-status production in the shared capability-printing helper. Raw capability retrieval may validly return an empty list; the caller requesting that report owns the user-facing error. |
| !2697 | Scanned | Lua DNS dissector example fixes root-zone and multi-question edge cases. |
| !2696 | Deep | Guy Harris makes Wireshark's -L/--list-tstamp-types path follow TShark's selected-interface option model, including authentication and monitor-mode state, instead of using a separate GUI interface inventory. |
| !2695 | Deep | Large RTP/VoIP performance refactor replaces repeated list scans with hashes, batches UI redraws, locks UI during retap, and reduces sample-list churn. Hot-path fixes address both algorithmic lookup cost and frontend update cost. |
| !2694 | Scanned | Adds HAVE_LIBPCAP protection around -k handling for builds without capture support; immediately refined by !2692. |
| !2693 | Deep | Windows compiler const-qualifier failure is fixed in both generated sysdig dissector output and tools/generate-sysdig-event.py. Generated-code fixes belong in the generator source as well as checked-in output. |
| !2692 | Deep | Guy Harris removes Wireshark-only -k from capture_opts_add_opt(); dumpcap and TShark do not share that semantic. Shared option parsers should contain only options genuinely shared by their consumers. |
| !2691 | Scanned | Fixes RTP Player Windows compilation; Guy Harris also corrects user-facing wording during review. |
| !2690 | Scanned | Fixes failure to advance the 802.11 Trigger Ranging parsing offset. |
| !2689 | Deep | RTP Player serializes playlist mutations against active operations and reports decode failures in the UI; reporter confirms the prior crash is no longer reproducible. |
| !2688 | Deep | Documentation symlink command is corrected only after the contributor demonstrates the broken and fixed paths with reproducible shell commands; concrete command output resolves an initially disputed docs change. |
| !2687 | Scanned | Indentation cleanup only. |
| !2686 | Deep | Guy Harris introduces a shared exit-code header and replaces duplicated/raw exit-code definitions across command-line frontends. |
| !2685 | Scanned | Clarifies rpcap preference wording so 'captured' cannot be confused with transport receipt by the rpcap client. |
| !2684 | Scanned | WSUG Tools menu text and screenshot update. |
| !2683 | Scanned | WSUG typo correction. |
| !2682 | Scanned | Windows Npcap dependency upgrade. |
| !2681 | Deep | MQTT dispatch tracks whether UAT or media-type decoding actually handled the payload and invokes heuristics only if neither did. Explicit configuration/content metadata takes precedence over heuristic guessing. |
| !2680 | Scanned | Adds colorized compiler diagnostics in CI/build tooling. |
| !2679 | Scanned | Updates RTP event type value mappings. |
| !2678 | Scanned | Corrects conditions and hierarchy for 802.11 Trigger Ranging Common/User Info. |
| !2677 | Scanned | Adds Trigger Ranging subtype detail to COL_INFO. |
| !2676 | Closed / down-weighted | John Thacker points out that a regex-based AsciiDoc image checker also scans commented-out references. The author closes the MR rather than add a brittle pre-commit rule that would need a real understanding of multiline comments. |
| !2675 | Deep | Protobuf loads .proto files from standard global/personal protobuf directories while retaining configurable search paths because tests and external schemas still rely on them. Add standard discovery without unnecessarily removing extensibility. |
| !2674 | Scanned | Large WSUG/RTP documentation refresh with related help links and images. |
| !2673 | Scanned | Automatic maintained-branch registry/data update. |
| !2672 | Scanned | Automatic release-3.4 registry/data update. |
| !2671 | Scanned | Automatic master registry/translation/data update. |
| !2670 | Deep | Before passing MQTT topic context through the heuristic dissector void* data contract, the code copies the const topic into packet-scope mutable storage instead of casting away const. Unknown callees must not be given writable access to caller-owned read-only memory. |
| !2669 | Scanned | Spelling checker gains recursive multi-word search and associated source spelling fixes. |
| !2668 | Scanned | RTP UI cleanup removes redundant menus and fixes opening Stream Analysis. |
| !2667 | Deep | Initializes RTP Analysis state before direct invocation, fixing a crash caused by use of an uninitialized structure. |
| !2666 | Scanned | F1AP 16.5.0 update keeps ASN.1/config/template inputs and generated packet-f1ap.c synchronized. |
| !2665 | Scanned | E1AP 16.5.0 update keeps ASN.1/config/template inputs and generated packet-e1ap.c synchronized. |
| !2664 | Scanned | XnAP 16.5.0 update keeps ASN.1/config/template inputs and generated packet-xnap.c synchronized. |
| !2663 | Deep | NGAP fix changes cnf/template source together with generated packet-ngap.c, reinforcing authoritative generated-source ownership. |
| !2662 | Scanned | Adds the 802.11 Ranging trigger type mapping. |
| !2661 | Deep | Guy Harris explicitly treats the per-linktype Packet Bytes split as a short-term prototype and argues for a general metadata/data architecture shared across encapsulations, potentially spanning libwiretap and libwireshark so tools such as editcap can translate metadata without dissectors. |

No SMPTE ST 291/VANC packet type was encountered in this batch.
