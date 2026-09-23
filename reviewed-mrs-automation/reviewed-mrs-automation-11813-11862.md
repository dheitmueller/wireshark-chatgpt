# Wireshark MR automation review: !11813–!11862

- **Corpus repository:** `dheitmueller/wireshark-corpus-mrs`
- **Corpus commit reviewed:** `ddcaa22b51c68f594e425a23388c3a2086813054`
- **Review direction:** descending MR number, newest available previously-unreviewed first
- **Exact MR count:** 50
- **Disposition:** 49 merged; 1 closed/unmerged (`!11851`)

## Selection and duplicate-avoidance

Before selecting this run, the available review tracking in `dheitmueller/wireshark-chatgpt` was reconciled, including `reviewed-mrs.md`, the supplemental automation tracker, and the per-run ledgers under `reviewed-mrs-automation/`. The historical `!17571`–`!17620` batch remains part of the already-reviewed set. The prior mention of `!11862` was only a frontier probe and was not counted as a review.

The exact selected set was:

`!11862, !11861, !11860, !11859, !11858, !11857, !11856, !11855, !11854, !11853, !11852, !11851, !11850, !11849, !11848, !11847, !11846, !11845, !11844, !11843, !11842, !11841, !11840, !11839, !11838, !11837, !11836, !11835, !11834, !11833, !11832, !11831, !11830, !11829, !11828, !11827, !11826, !11825, !11824, !11823, !11822, !11821, !11820, !11819, !11818, !11817, !11816, !11815, !11814, !11813`.

No numeric interval was assumed to be reviewed merely from partial ledger coverage.

## Durable notebook updates from this run

- `resource-lifetime-scope-conventions.md` — `!11837` establishes that allocator scope should match the logical reset boundary rather than merely using a convenient broader scope; `!11838` establishes that scoped containers/state still require explicit teardown for external-library resources. Commit `b8e986ed272937906fbc99e58d0bdc373843475e`.
- `api-contract-shaping-conventions.md` — `!11843` removes a result parameter the implementation never produced; `!11829` separates lookup status from result data and rejects string-pointer identity as value equality; `!11860` injects interface discovery into common capture-option code through a callback rather than coupling shared code to a frontend implementation. Commit `11f65af55a30f086ed464b867a5dead09816efc4`.
- `qt-object-lifetime-conventions.md` — `!11813` uses QObject parent ownership for dynamically allocated actions, `!11816`/`!11817` defer menu deletion until event processing is safe, and `!11857` moves invariant cleanup to the destructor so all close paths and nested event loops share one teardown boundary. Commit `750f66ecde3247df114929353e5f626339eb558f`.
- `installer-upgrade-cleanup-conventions.md` — `!11815` records that installer cleanup for files shipped by old releases may need to remain even after current releases stop installing those files, because upgrade-in-place can leave historical artifacts. Commit `8371b9f9ac9c348fcd24c53a9c4809a469685ee4`.
- `protocol-version-compatibility-conventions.md` — Guy Harris's merged `!11820` adds the rule that known deployed wire variants may both need recognition when the formal specification is incomplete/conflicting and credible implementation/history evidence supports them. Commit `d80499f86243845f8b6ce158c6d9b803a3c2e2e1`.
- `field-decoding-api-conventions.md` — `!11819` records that normalized decoder output must be validated before fixed-position access, and superficially adjacent fields should not be forced through the same decoder when their encodings differ. Commit `5c9c40c6d799d0048c9ea1fc1f41d89491062664`.

## Per-MR review ledger

| MR | State | Review result |
| --- | --- | --- |
| !11862 | merged | PAPI/Aruba License Manager dispatch is moved into the normal registered dissector/internal-port table path instead of bespoke source/destination checks; useful corroboration for table-driven dispatch. |
| !11861 | merged | GitHub/MSYS2 CI now builds an NSIS installer artifact, installs it in a separate job, and exercises the installed Wireshark; strong end-to-end artifact validation evidence. |
| !11860 | merged | Guy Harris: common capture-option code receives an interface-list callback rather than directly depending on frontend-specific discovery; promoted to API/architecture guidance. |
| !11859 | merged | Release-3.6 donation-page wording backport; scanned, no distinct durable convention. |
| !11858 | merged | Release-4.0 donation-page wording backport; scanned, no distinct durable convention. |
| !11857 | merged | Gerald Combs moves Manage Interfaces cleanup into the destructor to avoid early teardown during close/nested-event processing; promoted to Qt lifetime guidance. |
| !11856 | merged | Guy Harris: `-D` must report local interface-list failure even if extcap/remote interfaces make the aggregate list nonempty; strong error-contract corroboration. |
| !11855 | merged | Spelling-only cleanup; scanned. |
| !11854 | merged | Automatic services-data update; generated maintenance change, not promoted. |
| !11853 | merged | Manuf generator sanitizes backslashes in company names; narrow data-generation fix. |
| !11852 | merged | Donation/UI maintenance; scanned, no cross-cutting rule. |
| !11851 | closed | Failed/superseded automatic data update; down-weighted as unmerged and represented by the succeeding merged update. |
| !11850 | merged | Release-4.0 automatic data update; scanned. |
| !11849 | merged | Release-3.6 automatic data update; scanned. |
| !11848 | merged | UI title is derived from current application state rather than piecemeal transition updates; useful state-derived-UI corroboration. |
| !11847 | merged | TCP Fast Retransmission documentation correction; narrow protocol-analysis documentation fix. |
| !11846 | merged | Guy Harris separates interface-info lookup from mutation/filling and centralizes the mutation at the add-interface boundary; useful cohesion/API evidence. |
| !11845 | merged | MSYS2 packaging uses version-tolerant DLL globbing instead of hardcoded sonames; packaging maintenance evidence. |
| !11844 | merged | Guy Harris documents how local, remote, and extcap interface lists are composed; corroborates the failure-boundary lesson in !11856. |
| !11843 | merged | Guy Harris removes an `err_str` output parameter that the extcap helper never populated; promoted as an API-contract-shaping rule. |
| !11842 | merged | Guy Harris cleans up sync-pipe write APIs with semantic operation-specific names/helpers and narrows low-level details to file scope; useful encapsulation evidence. |
| !11841 | merged | Aruba UBT cleanup/fixes; scanned, no separate cross-cutting rule. |
| !11840 | merged | Release-note formatting/escaping fix; scanned. |
| !11839 | merged | DOCSIS 4.0 TLV update against current specification; protocol maintenance, no new generic rule. |
| !11838 | merged | John Thacker ties libgcrypt cipher-handle teardown to file-scope state cleanup; promoted as external-resource lifetime guidance. |
| !11837 | merged | John Thacker introduces a dedicated wmem lifetime matching address-resolution reset semantics instead of leaking repeated epan-scope generations; promoted. |
| !11836 | merged | Replaces `GTimer` with monotonic-time measurement in dftest; straightforward simplification. |
| !11835 | merged | HCI monitor logging contribution cites Linux kernel/BlueZ implementation sources where formal protocol documentation is absent; useful submission/review provenance evidence. |
| !11834 | merged | Restores JSON dumper indentation for nested closing elements; regression fix, reviewed/scanned. |
| !11833 | merged | Gerald Combs allows manual source-package CI execution; CI workflow maintenance. |
| !11832 | merged | Version bump 4.1.0 to 4.1.1; release bookkeeping. |
| !11831 | merged | CAN cleanup/style/typos; scanned. |
| !11830 | merged | 4.1.0 build/release-note/ChangeLog preparation; release bookkeeping. |
| !11829 | merged | Peter Wu + João Valverde: constifies manuf tables, makes lookup/iteration return explicit success status, hides implementation details, and avoids string-pointer identity; promoted. |
| !11828 | merged | FlexRay cleanup/style consistency; scanned. |
| !11827 | merged | Lua console surfaces compilation syntax errors and makes output read-only; useful diagnostic/UI evidence without a distinct new rule. |
| !11826 | merged | Adds MKA CKN name resolution via UAT/hash state for static automotive deployments; feature work, no separate generic convention. |
| !11825 | merged | HTTP/3/nghttp3 integration carried forward from an earlier MR; substantial dependency/review discussion, but no stronger new rule than existing notebook guidance. |
| !11824 | merged | Documents `proto_deregister_protocol()` as internal to Lua-plugin reload and not for dissectors/plugins; useful API-boundary documentation evidence. |
| !11823 | merged | DHCP indentation cleanup; scanned. |
| !11822 | merged | Guy Harris spelling correction in PGM source commentary; scanned. |
| !11821 | merged | Adds CableLabs DHCP vendor-class IDs with specification references; standards-backed protocol update. |
| !11820 | merged | Guy Harris accepts both known PGMCC ACK packet-type encodings because the specification/history is incomplete/conflicting; promoted to protocol compatibility guidance. |
| !11819 | merged | John Thacker validates BCD-decoder output before indexing and decodes timezone separately because its representation differs; promoted to field-decoding guidance; Guy Harris review weighted highly. |
| !11818 | merged | Guy Harris replaces same-type struct `memcpy` with direct C structure assignment; high-authority but narrow C-style evidence. |
| !11817 | merged | Release-4.0 backport of the context-menu deferred-deletion fix; corroborates master !11816. |
| !11816 | merged | Gerald Combs uses `deleteLater()` rather than immediate close deletion for a transient QMenu involved in signal dispatch; promoted to Qt lifetime guidance. |
| !11815 | merged | João Valverde with Gerald Combs review: retain installer cleanup for files installed by older versions so upgrade-in-place removes stale artifacts; promoted. |
| !11814 | merged | DoIP recognizes protocol version `0xff` for the specified vehicle-identification case; protocol compatibility fix, no separate generic rule. |
| !11813 | merged | João Valverde parents dynamically allocated QActions to the table view so QObject ownership releases them; promoted to Qt lifetime guidance. |

Merged master work was weighted above backports and the single closed/superseded MR. High-authority comments and authored changes from Guy Harris, Gerald Combs, John Thacker, João Valverde, and Peter Wu were weighted according to the substance of the discussion and accepted implementation rather than name alone.
