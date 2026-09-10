# Reviewed Wireshark Merge Requests !25109–!25158

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: descending MR number. This ledger records exactly the 50 previously unreviewed MRs selected for this run. The previously reviewed historical batch !17571–!17620 remains part of the already-reviewed set and was preserved when selecting this batch.

| MR | Status | Review notes |
|---|---|---|
| !25158 | Deep, merged | Reverted the Find in Packet feature before the 4.7.0 test/release window because known Stratoshark problems and incompleteness made a narrow crash fix insufficient. Accepted outcome strongly supports reverting an insufficiently ready cross-application feature rather than shipping a partial band-aid late in the cycle. |
| !25157 | Discussion-focused, closed/superseded | Proposed a minimal Stratoshark crash fix for Find in Packet. John Thacker preferred reverting the feature until after the 4.7.0 test release; the author agreed and !25158 became the accepted outcome. Lower implementation weight because this MR was closed. |
| !25156 | Scanned, merged | Profile dialog fix keeps selection semantics correct when the special Default profile is reset rather than actually deleted. Reinforces special-object lifecycle/state handling; no new general rule extracted. |
| !25155 | Scanned, merged | Windows WinSparkle package/layout update with version discovery. Packaging maintenance; no new durable Wireshark convention beyond existing dependency/platform guidance. |
| !25154 | Scanned, merged | NAS-5GS URSP Location criteria route-selection descriptor support. Protocol-extension work; no novel review convention extracted. |
| !25153 | Scanned, merged | Haiku socket users explicitly link libnetwork. Platform-specific dependency correction; reinforces platform API/link semantics already captured. |
| !25152 | Scanned, merged | Continued conversion of dissectors to avoid separate fetches when tree APIs can return decoded values. Reinforces existing parser/API guidance on single authoritative extraction. |
| !25151 | Scanned, merged | Registers and parses previously missing NAS-5GS URSP component types. Straightforward protocol coverage extension. |
| !25150 | Scanned, merged | Adds NAS-5GS URSP regular-expression traffic-descriptor parsing. Straightforward protocol coverage extension. |
| !25149 | Scanned, merged | Adds URSP time-window, PDU-session-pair-ID, and RSN descriptors. Straightforward protocol coverage extension. |
| !25148 | Scanned, merged | Haiku format-warning fix accounts for the platform's actual `ssize_t`/`SSIZE_MAX` types. Reinforces ABI/type-aware portability guidance. |
| !25147 | Scanned, merged | Fixes an off-by-one in NAS-5GS URSP Destination FQDN decoding. Reinforces exact length/offset accounting. |
| !25146 | Scanned, merged, Guy Harris-authored | Adds required Haiku headers in recently moved/added code. High-authority portability evidence, but too narrow to justify a new general rule. |
| !25145 | Scanned, merged | Doxygen conversion for Wireshark headers. Documentation-only maintenance. |
| !25144 | Scanned, merged | Broader Haiku portability changes. Useful platform corroboration, but existing portability/build guidance already covers the durable lesson. |
| !25143 | Deep, merged | UET CRC support separates out-of-band CRC-presence configuration (`has_crc`) from optional computation/validation (`validate_crc`), with focused captures and fuzz testing. Good validation/test evidence; no broader rule promoted because checksum/configuration guidance is already represented elsewhere. |
| !25142 | Deep, merged | Nested Ethernet dissection of IPFIX `dataLinkFrameSection` changed `pinfo->ptype/srcport/destport`; later cflow template lookup then used corrupted outer context. Fix saves/restores port context, complementing existing address restoration, and includes before/after tshark evidence. Strong corroboration of scoped `packet_info` restoration. |
| !25141 | Deep, merged, John Thacker-authored | Follow payload selection belongs in the listener when one frame may contain multiple logical streams and multiple follow taps may coexist; the dissector cannot know which stream a listener wants. Strong tap/context design evidence, consistent with existing capture-at-production stream identity guidance. |
| !25140 | Scanned, merged | Corrects NL80211/802.11 handling where tag value zero can mean SSID rather than padding depending on context. Reinforces context-sensitive interpretation rather than value-only dispatch. |
| !25139 | Scanned, merged backport | release-4.4 backport of the CSN_UINT_ARRAY pointer-increment fix. Same accepted one-line correctness fix as !25138; no additional lesson. |
| !25138 | Scanned, merged backport | release-4.6 backport fixes `*pui8++` sequencing so the tree displays the value just decoded rather than the next stale/uninitialized slot. Reinforces explicit sequencing when a value is both stored and immediately consumed. |
| !25137 | Scanned, merged | Adds a buffer-boundary check to FreeBSD USB frame dissection. Reinforces existing captured-length/bounds conventions. |
| !25136 | Scanned, merged | Debian packaging avoids libcpuinfo dependency on architectures the library itself does not support even where a package exists. Reinforces capability/target-aware dependency selection. |
| !25135 | Scanned, merged | More dissector conversions avoiding redundant value fetches. Reinforces existing parser/API extraction guidance. |
| !25134 | Scanned, merged backport | Backport of C2P sub-100ms timestamp-formatting correction. No new lesson beyond the master fix. |
| !25133 | Scanned, merged | Qt generated-header include/checkAPI cleanup. Narrow style/tooling conformance. |
| !25132 | Scanned, merged | Restores executable permission for a VMware heartbeat generator. Repository/tooling metadata fix. |
| !25131 | Scanned, merged | Adds Rel-18 NAS-5GS URSP connection-capability values. Data/specification update. |
| !25130 | Scanned, merged | Weekly update tooling gains a target-branch option because ASTERIX specification regeneration is master-only. Reinforces branch-aware automation. |
| !25129 | Scanned, merged | Corrects swapped SUSE/RHEL Qt SVG package names. Packaging metadata correction. |
| !25128 | Scanned, merged | Keeps the special Default profile at the front of the ProfileModel even through reset/delete semantics. Reinforces explicit invariants for sentinel/special model entries. |
| !25127 | Deep, merged | DNS SVCB/HTTPS semantic validation warns about duplicate/out-of-order keys and cross-parameter violations while deliberately preserving malformed handling, resynchronization, and continued dissection. Focused crafted capture verifies both diagnostics and lack of parser desync. Strong corroboration of non-destructive semantic validation. |
| !25126 | Scanned, merged | CMake discovers/links QtSvg needed by welcome-page SVG content. Build dependency completeness; later packaging fixes refine platform package names. |
| !25125 | Scanned, merged | Weekly-update module list is deduplicated and sorted for stable tooling behavior. Reinforces deterministic generated/automation inputs. |
| !25124 | Scanned, merged | Doxygen conversion for Wireshark headers. Documentation-only maintenance. |
| !25123 | Scanned, merged | Adds a VMware release-data update utility. Tooling addition; no review pattern strong enough for promotion. |
| !25122 | Scanned, merged | CSN1 M_UINT_ARRAY output now shows element values rather than only the element count. Presentation correctness fix. |
| !25121 | Scanned, merged | Kafka compact-array length is decoded as the specified 32-bit VARINT rather than a 64-bit VARLONG. Reinforces using the protocol's exact encoded integer domain. |
| !25120 | Scanned, merged | Lua debugger Qt checkAPI cleanup. Narrow include/API conformance. |
| !25119 | Scanned, merged | Corrects OpenSUSE runtime QtSVG package dependency. Packaging follow-up. |
| !25118 | Scanned, merged | Spelling corrections. No engineering convention extracted. |
| !25117 | Scanned, merged | Fixes C2P millisecond display zero-padding for values below 100 ms. Presentation correctness; !25134 is the backport. |
| !25116 | Scanned, merged | Lua debugger Qt checkAPI cleanup. Narrow include/API conformance. |
| !25115 | Scanned, merged | Large Doxygen conversion for public headers, including ownership/lifetime descriptions. Documentation maintenance; no new implementation rule extracted. |
| !25114 | Scanned, merged | WelcomePage initially hides the sidebar and lets established preference/application state reveal it during initialization. Reinforces avoiding premature UI state before preferences are applied. |
| !25113 | Scanned, merged | Debian setup handles Ubuntu Jammy's distinct Qt6 SVG development package name. Platform packaging compatibility. |
| !25112 | Scanned, merged | Makes QtSvg an explicit required/link dependency for SVG-bearing UI content. Build dependency completeness. |
| !25111 | Scanned, merged | Removes obsolete macOS Brotli install-name workaround now that upstream Brotli fixed it. Reinforces deleting compatibility hacks once their scoped upstream condition no longer exists. |
| !25110 | Scanned, merged | O-RAN FH CUS UI/tree presentation explicitly indicates when UL configuration causes `udCompHdr` settings to be ignored. Improves visibility of effective configuration; no new general rule extracted. |
| !25109 | Deep, merged, John Thacker-authored | `NL80211_ATTR_FRAME` normally contains an 802.11 frame, but under `NL80211_CMD_CONTROL_PORT_FRAME` it is raw EAPoL with no Ethernet/802.11 header. The fix carries command context into attribute dissection and dispatches to EAPoL only for that command, with a sample capture. Strong corroboration of context-driven child dissector selection. |

## Durable conclusions

* **Release readiness can outweigh a narrow crash fix.** !25157 proposed making an incomplete feature merely stop crashing in Stratoshark. John Thacker recommended reverting it before the 4.7.0 test release, and merged !25158 implemented the full revert. When a cross-application feature is known incomplete near a release boundary, prefer restoring the last known-good feature set over layering a minimal safety patch that leaves incomplete behavior exposed.
* !25142 strongly corroborates the existing rule that nested dissectors which mutate shared `packet_info` state must restore all outer-layer fields whose values remain semantically owned by the caller.
* !25141 corroborates recording/propagating logical stream identity to consumers that can distinguish multiple streams within one frame; filtering in the producer is insufficient when listeners have independent stream selections.
* !25127 corroborates that semantic protocol violations can be diagnosed without intentionally breaking parse synchronization when the wire structure remains safely decodable.
* !25109 corroborates that child payload type may depend on parent operation/command context even when the enclosing attribute identifier is the same.

No more than 50 MRs were reviewed in this run.