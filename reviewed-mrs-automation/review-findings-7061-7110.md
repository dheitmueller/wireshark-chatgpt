# Review findings: !7061–!7110

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is weighted more heavily than abandoned work. Maintainer review is called out when it materially strengthens a conclusion. Closed !7080 and !7063 are retained only as historical/negative evidence.

| MR | Outcome | Depth | Findings |
|---|---|---|---|
| !7110 | merged | Scanned | CMake/NSIS version-variable cleanup; reduces duplicate aliases and aligns generated installer variables with project-version names. |
| !7109 | merged | Discussion-focused | Documents that display-filter layer indices start at 1. João Valverde confirmed the semantics and asked that the filter manual carry the clarification. |
| !7108 | merged | Scanned | Removes a now-impossible null check after the conversation API began normalizing null addresses at entry; Coverity exposed the stale invariant check. |
| !7107 | merged | Scanned | Deduplicates project/version variables across packaging templates and generated resources; straightforward build-system cleanup. |
| !7106 | merged | Deep | FC ELS correction after conversation-option split: lookup uses NO_*_B query wildcards while creation uses NO_*2 stored-endpoint omission flags. |
| !7105 | merged | Deep | UMTS FP correction for the same conversation-option distinction: find_conversation and conversation_new require different option domains. |
| !7104 | merged | Deep | RTP heuristic lookup switches to NO_ADDR_B for search while creation retains NO_ADDR2 semantics; corroborates the conversation API contract. |
| !7103 | merged | Deep | JXTA conversation creation switches from search-only NO_PORT_B to creation-side NO_PORT2. |
| !7102 | merged | Deep | SOME/IP-SD option parsing now checks each nested option against both captured TVB bytes and the enclosing declared option-array length. |
| !7101 | merged | Deep | T.38 conversation creation uses NO_ADDR2|NO_PORT2 rather than search-wildcard flags. |
| !7100 | merged | Deep | CoAP conversation creation uses NO_ADDR2|NO_PORT2 after the core API split; accepted caller-audit follow-up. |
| !7099 | merged | Deep | ZigBee ZCL feature MR later received Stig Bjørlykke post-merge review identifying an indexed ett array too small for its loop and missing subtree registration; later fixed in !13341. Strong negative structural-testing evidence. |
| !7098 | merged | Scanned | Updates generated introspection enums and makes the generator usable with repository-default input/output paths. |
| !7097 | merged | Discussion-focused | LLDP/CIP TLVs. Alexis La Goutte requested a sample pcap; contributor supplied one. Gerald Combs requested a default switch case; contributor added it. |
| !7096 | merged | Scanned | Conversation/endpoint dialogs automatically reflect an already-active display filter in their initial UI state. |
| !7095 | merged | Scanned | GTP QoS bitrate field corrections and clearer formatted labels; protocol-specific presentation/correctness work. |
| !7094 | merged | Scanned | Travelping PFCP vendor-IE update; only minor style review, no new durable convention. |
| !7093 | merged | Deep | Traffic tables keep total and display-filter-selected populations in one model/tap path, with filtering performed by a proxy model and explicit row match state. |
| !7092 | merged | Scanned | Corrects exact DCT2000 NR-PDCP log string formats used to discover keys; narrow parser compatibility fix. |
| !7091 | merged | Scanned | Fixes NSIS include after config file rename; build/packaging consistency repair. |
| !7090 | merged | Discussion-focused | John Thacker removes conversation symbols from Debian ABI symbol tracking when implementations were removed, and deletes a leftover public declaration. |
| !7089 | merged | Deep | Conversation lookup normalizes null address pointers to AT_NONE at the API boundary and documents that contract. |
| !7088 | merged | Deep | SOME/IP UAT/cache cleanup adds type-specific deep destructors, frees nested allocations, nulls released record pointers, and uses zero-initializing wmem allocation helpers. |
| !7087 | merged | Scanned | Stable-branch Qt codec crash fix: available codec IDs do not guarantee backend availability; codecForMib can return null and must be checked. |
| !7086 | merged | Scanned | Master counterpart of the Qt/ICU codec availability fix for Wireshark/Logwolf. |
| !7085 | merged | Deep | Signal-PDU profile reload performance: dynamically register optional aggregation fields only when configured; 150k-signal unregister time had regressed from about 50 s to 592 s. Review also replaced a magic count with a named constant. |
| !7084 | merged | Scanned | Automatic translation/data update; no substantive reusable human-review lesson. |
| !7083 | merged | Scanned | Automatic registry/data update; no substantive reusable human-review lesson. |
| !7082 | merged | Scanned | Automatic registry/data update; no substantive reusable human-review lesson. |
| !7081 | merged | Deep | NVMe replaces ad-hoc integer-to-pointer casts with GLib GUINT_TO_POINTER/GPOINTER_TO_UINT macros when storing a guint32 in gpointer-valued structures. |
| !7080 | closed | Scanned (closed) | Proposed common 802.11 radio bandwidth filter field was closed unmerged and had no substantive review discussion; not treated as implementation precedent. |
| !7079 | merged | Deep | Signal-PDU UAT free callback now releases all owned strings and clears the pointers, corroborating full record-ownership cleanup. |
| !7078 | merged | Scanned | Moves traffic-table context-menu behavior into a dedicated TrafficTree widget; UI responsibility consolidation with no strong review discussion. |
| !7077 | merged | Deep | CI explicitly enables and builds tfshark although it is disabled by default; maintained non-default targets need continuous build coverage or they can silently rot. |
| !7076 | merged | Deep | Version reporting stops claiming a loaded plugin count before EPAN/plugin initialization; separates compiled plugin capability from runtime platform support. |
| !7075 | merged | Discussion-focused | WoW field width fix. Martin Mathieson ran check_typed_item_calls.py and exposed additional FT_UINT8/4-byte call mismatches, reinforcing typed-item structural checking. |
| !7074 | merged | Scanned | 3.6 FlexRay backport avoids calling tvb_bytes_to_str_punct with zero payload length, preventing an exception on empty payloads. |
| !7073 | merged | Deep | John Thacker narrows Follow Stream preconditions: a selected packet dissection is required only for packet-driven following, not when the user is navigating by known stream index. |
| !7072 | merged | Discussion-focused | Gerald Combs adopts standard CMake user presets after João Valverde points to the upstream mechanism; local CMakeUserPresets.json is ignored and documented. |
| !7071 | merged | Scanned | Adds Logwolf-specific NSIS configuration alongside Wireshark packaging; substantial packaging work but no reusable review correction. |
| !7070 | merged | Deep | Small unsigned arithmetic is integer-promoted: an 8-bit wraparound test must account for promotion rather than assuming uint8 operands wrap before comparison. |
| !7069 | merged | Scanned | Normalizes NSIS indentation to the upstream project's two-space convention. |
| !7068 | merged | Scanned | Improves traffic-table retap lifecycle and UI handling; no substantive human review in the corpus snapshot. |
| !7067 | merged | Discussion-focused | Packaging target/file renames are carried through CI and docs. Gerald Combs states a consistent <application>_<type> target naming direction. |
| !7066 | merged | Scanned | Renames time-display wording to match actual semantics and updates UI plus user documentation together. |
| !7065 | merged | Scanned | Developer documentation drops Bison/YACC after the last parser dependency was removed; dependency docs track the actual build graph. |
| !7064 | merged | Deep | Core conversation API separates creation flags (NO_ADDR2/NO_PORT2) from search wildcard flags (NO_ADDR_B/NO_PORT_B), adds assertions, and triggers caller fixes. Stig Bjørlykke explicitly noted current dissectors also needed auditing. |
| !7063 | closed | Scanned (closed) | No meaningful diff was present; Tomasz Moń asked what was intended to be merged. Closed unmerged and carries no implementation precedent. |
| !7062 | merged | Scanned | Qt TrafficView Coverity cleanup handles tap-filter error strings and state more safely; static-analysis-driven maintenance. |
| !7061 | merged | Scanned | Suppresses known narrowing warnings in bundled lrexlib under MSVC; third-party compatibility-specific change. |

## Validation

- Rows: 50
- Unique MR numbers: 50
- Highest: !7110
- Lowest: !7061
- Closed/unmerged: !7080, !7063
