# Reviewed Wireshark Merge Requests !25009–!25058

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: descending MR number. This ledger records exactly the 50 highest-numbered previously unreviewed MRs selected after consulting all available `reviewed-mrs-automation/` ledgers, the supplemental automation ledger, and `reviewed-mrs.md`. The previously reviewed historical batch !17571–!17620 remains part of the already-reviewed set and was preserved when selecting this batch.

| MR | Status | Review notes |
|---|---|---|
| !25058 | Scanned, merged | Windows CI/Visual Studio 2026 migration. Build maintenance; no new durable rule. |
| !25057 | Scanned, merged, John Thacker-authored | Adds nl80211 field generation to weekly updates. Reinforces automation of generated protocol data. |
| !25056 | Scanned, merged, John Thacker-authored | WiX uses the authoritative MSVC toolset-version variable rather than another inferred value. Reinforces existing single-source build guidance. |
| !25055 | Deep, merged, John Thacker-authored | Adds a zero-length nl80211 protocol item for payloadless commands so protocol presence remains filterable. Useful protocol-tree/filterability corroboration. |
| !25054 | Scanned, merged | Parallelizes `check_apis.py` whole-tree processing. Tooling performance improvement; no architectural rule promoted. |
| !25053 | Deep, open | WSLua Listener self-removal UAF proposal. Stig Bjørlykke preferred deferred removal over banning a legitimate callback action. Kept provisional because the MR remained open. |
| !25052 | Scanned, merged, John Thacker-authored | DCT2000 caps a capture-controlled entry count to fixed destination capacity. Reinforces existing hostile-count bounds guidance. |
| !25051 | Scanned, merged | Weekly-update PCI generator path correction. Tool maintenance only. |
| !25050 | Scanned, merged | CMake/WiX merge-module path correction. Build maintenance only. |
| !25049 | Scanned, merged | GSM A/RLCMAC uses semantic formatting for encoded units rather than ad-hoc value tables. Reinforces presentation/semantic-unit guidance. |
| !25048 | Deep, merged | WSLua CaptureInfoConst accesses the correct reader/writer-owned section-header source and adds coverage. Reinforces ownership/source-object contracts. |
| !25047 | Deep, closed, Guy Harris review | Proposed defensive NULL-handle behavior was challenged because `wtap_fdclose()` represents a narrow temporary-close state, not a general closed-object contract. Down-weighted as unmerged but retained as strong architectural caution. |
| !25046 | Deep, merged | NVMe-MI request/response tracking uses first-pass state mutation, per-frame transaction references, and an interim MPR state before final completion. Reinforces redissection-safe transaction state. |
| !25045 | Scanned, merged backport | pcapng NRB custom-string copy uses the string body length rather than the full option size including PEN. Reinforces semantic length domains. |
| !25044 | Deep, merged | USB PTP generator remains manually vetted because upstream source values contain known typos. Automation should not blindly ingest unreliable upstream generated data. |
| !25043 | Deep, merged, Gerald Combs-authored | CMake supplies `MSVC_TOOLSET_VERSION` directly instead of reconstructing it from compiler versions. Strong single-source-of-truth corroboration. |
| !25042 | Deep, merged | ETW warning cleanup exposed a callback argument semantic bug; accepted code centralizes hash-table value destruction and replacement ownership. Promoted with !25029. |
| !25041 | Deep, merged backport, John Thacker-authored | Netlog heuristic open validates that required `events` structure exists and rejects nonmatching input instead of crashing. Corroborates !25037. |
| !25040 | Deep, merged, John Thacker-authored | Generic-netlink dynamic family mappings retain frame-aware history and mark defining frames as dependencies for export. Promoted to capture-state history conventions. |
| !25039 | Scanned, merged, John Thacker-authored | Master pcapng NRB custom-string copy-length fix. Same semantic-length lesson as !25045. |
| !25038 | Scanned, merged, Gerald Combs-authored | Visual Studio 2026 CI migration; John Thacker caught remaining `.slnx` artifact references. Migration consistency only. |
| !25037 | Deep, merged, John Thacker-authored | Netlog reader treats missing required JSON structure as NOT_MINE/heuristic rejection rather than dereferencing absent state. Reinforces input-shape validation. |
| !25036 | Scanned, merged | Documentation screenshot switches GIF to PNG; review suggested PNG compression. Documentation/media maintenance. |
| !25035 | Scanned, merged | Lua debugger clear-button/documentation change. No durable engineering convention extracted. |
| !25034 | Scanned, merged | Visual Studio 2026 compiler-version reporting update. Build/version maintenance. |
| !25033 | Scanned, merged backport | Stable-branch compiler-version reporting update. Corroborates !25034. |
| !25032 | Scanned, merged, John Thacker-authored | nl80211 generator can run from any directory and updates output by default for weekly automation. Tool ergonomics/automation. |
| !25031 | Scanned, merged, John Thacker-authored | `.editorconfig` records the actual tab convention for netlink-generic source. Style codification. |
| !25030 | Scanned, merged, John Thacker-authored | MSVC 2026 version-info mapping. Build/version maintenance. |
| !25029 | Deep, merged | Funnel menu removal now unlinks the list element before freeing it, fixing a use-after-free. Promoted ownership/mutation ordering with !25042. |
| !25028 | Scanned, merged backport, John Thacker-authored | Qt resource-generation compatibility/reproducibility fixes for newer Qt on Windows. Existing build-portability guidance. |
| !25027 | Deep, merged | Weekly-update gains script arguments; Jaap Keuter requested documentation and stale-comment cleanup before merge. Reinforces documenting new tooling interfaces. |
| !25026 | Deep, merged, John Thacker-authored | Restores `packet-netlink-nl80211.c` because the file contains actual dissection, unlike data-only generated sources. Promoted source-layout naming convention. |
| !25025 | Deep, merged, John Thacker-authored | Weekly-update launches Python helpers with `sys.executable`, fixing Windows execution and preserving interpreter/environment identity. Promoted tooling convention. |
| !25024 | Scanned, merged | User Guide documents additional menu items. Documentation maintenance. |
| !25023 | Scanned, merged backport, Gerald Combs-authored | CI variable moved into `before_script` because `extends` changed inherited global-variable behavior. CI inheritance caution. |
| !25022 | Deep, merged | ISOBUS BAM now initializes the same reassembly state as RTS; John Thacker requested a sample capture before merge. Reinforces complete reassembly setup and capture-based validation. |
| !25021 | Deep, merged, John Thacker-authored | TCP Follow tap filters by explicit stream ID because a single frame can involve more than one TCP stream. Strong corroboration of existing stream-identity guidance. |
| !25020 | Deep, merged, John Thacker-authored | Protocol-tree end/appendix APIs assert that relative offsets use the same backing data source as the field. Promoted range/data-source contract. |
| !25019 | Scanned, merged | Exposes Lua release information to WSLua; review included local style correction. API maintenance only. |
| !25018 | Scanned, merged | Adds UM 6.17 ranged retransmission request dissection with an attached sample capture. Good submission/testing example; no new rule needed. |
| !25017 | Deep, merged, John Thacker-authored | OSS-Fuzz WCCP fix uses `proto_item_set_end()` instead of redundant accumulated signed length, avoiding UB overflow. Promoted protocol-tree range rule. |
| !25016 | Deep, merged, John Thacker-authored | Normalizes/warns about POSIX stack limits below Wireshark's tested 8 MiB baseline. Useful supported-runtime/resource-contract evidence, but too environment-specific for a new general rule. |
| !25015 | Scanned, merged | Adds UM 6.17 LBMR source flags and a typo cleanup. Straightforward protocol coverage. |
| !25014 | Deep, merged, John Thacker-authored | TCP OOO state keeps ordered and hashed access paths over the same objects for faster later-pass lookup, with sequence-order invariants documented. Strong data-structure/performance example. |
| !25013 | Scanned, merged, Gerald Combs-authored | Master version of CI variable-clobber fix later backported in !25023. CI inheritance behavior. |
| !25012 | Deep, merged, John Thacker-authored | SCSI SBC uses `proto_item_*_ret_uint*` to avoid double fetches, fixes signed domains, and removes overflow-prone arithmetic. Reinforces existing extraction/arithmetic conventions. |
| !25011 | Scanned, open | HL7 documentation-reference correction; Anders Broman requested whitespace cleanup, conventional commit wording, and squashing. Kept lower-weight because still open. |
| !25010 | Deep, merged | Follow-up fixes issues found by `check_dissector` after CVG review, reinforcing mechanical checker value and complete registration. |
| !25009 | Deep, merged | DECT NR CVG heuristic requires positive length and registers heuristic handling in the proper handoff phase. Reinforces heuristic invariants/registration lifecycle. |

## Durable conclusions

* **Capture-learned mappings that can change need historical lookup and export dependencies.** !25040 retains generic-netlink family mapping history by frame and marks defining packets as dependencies so exported subsets remain decodable.
* **Invoke Python child tools with the current interpreter.** !25025 uses `sys.executable`, avoiding Windows executable-file assumptions and accidental interpreter/environment changes through `PATH`.
* **Source-file naming should reflect actual responsibility.** !25026 keeps a mixed/generated source as `packet-*` while it still contains dissection logic; a `data-*` classification should follow a real responsibility split.
* **Unlink before destruction, and encode ownership in containers where possible.** !25029 directly fixes a UAF by removing a list element before freeing it; !25042 complements this by using hash-table destroy/replacement semantics rather than manual ownership sequences.
* **Prefer endpoint-based tree ranges and preserve data-source identity.** !25017 avoids overflow by setting an already-known item end rather than accumulating a redundant length; !25020 asserts that relative range adjustments refer to the field's same underlying data source.
* !25047 is intentionally retained only as lower-weight negative evidence: Guy Harris's review warns against broad defensive behavior that turns a specialized transitional wiretap state into an undocumented general API state, but the MR was closed rather than merged.

Exact reviewed MR numbers: !25058, !25057, !25056, !25055, !25054, !25053, !25052, !25051, !25050, !25049, !25048, !25047, !25046, !25045, !25044, !25043, !25042, !25041, !25040, !25039, !25038, !25037, !25036, !25035, !25034, !25033, !25032, !25031, !25030, !25029, !25028, !25027, !25026, !25025, !25024, !25023, !25022, !25021, !25020, !25019, !25018, !25017, !25016, !25015, !25014, !25013, !25012, !25011, !25010, !25009.

Exactly 50 MRs were reviewed in this run; no MR below !25009 was included.