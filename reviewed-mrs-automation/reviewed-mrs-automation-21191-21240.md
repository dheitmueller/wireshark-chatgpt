# Wireshark MR review automation ledger — !21191 through !21240

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Direction: newest available previously-unreviewed MRs toward older MRs.

This run reviewed exactly **50** previously unreviewed merge requests. Selection was rebuilt from `reviewed-mrs.md`, the aggregate automation ledger, and the per-run files in `reviewed-mrs-automation/`; numeric ranges were not assumed complete. The previously reviewed historical !17571-!17620 batch remains part of the already-reviewed set and was not revisited. The prior automation frontier ended at !21241, so the next fifty corpus MRs are the contiguous set !21240 through !21191.

## Exact MRs reviewed

`!21240 !21239 !21238 !21237 !21236 !21235 !21234 !21233 !21232 !21231 !21230 !21229 !21228 !21227 !21226 !21225 !21224 !21223 !21222 !21221 !21220 !21219 !21218 !21217 !21216 !21215 !21214 !21213 !21212 !21211 !21210 !21209 !21208 !21207 !21206 !21205 !21204 !21203 !21202 !21201 !21200 !21199 !21198 !21197 !21196 !21195 !21194 !21193 !21192 !21191`

Count: **50**.

## Durable findings promoted

- **!21228 — deep, merged master; Gerald Combs authored/approved/merged.** `make-enums.py` now compares the proposed generated output with the existing artifact and refuses to overwrite it when a buggy PyClibrary unexpectedly produces fewer lines. Stable variants !21233-!21235 carry the same guard. Promoted to `generated-output-conventions.md`: successful generator execution is not sufficient evidence that output is plausible; cheap structural invariants should prevent publishing catastrophically incomplete generated artifacts.
- **!21215 — deep, merged master; Michael Mann authored/approved/merged.** K12 format-open diagnostics that occur during normal probing of non-K12 files move from warning to debug severity. Promoted to new `wiretap-format-detection-conventions.md`: `WTAP_OPEN_NOT_MINE`-style rejection is an expected part of multi-format detection and should not generate user-visible warning noise merely because a candidate opener declined the file.

## Strong corroborating findings

- **!21222 — very high weight, merged master; John Thacker authored and merged.** HTTP must not retain a `pinfo->pool`-scoped map inside file-scope request/response state. The fix creates file-scope storage before persisting it. This strongly corroborates the notebook's existing allocator/lifetime rule that a container or pointer cannot outlive the allocator scope backing its contents.
- **!21200 — very high weight, merged master; John Thacker authored, Anders Broman approved/merged.** MKA stops treating `pinfo->private_table` as dissector-owned storage and uses scoped protocol data instead, avoiding interference with Lua/other users and avoiding explicit cleanup across dissector exceptions. This is already captured by the later !22163 evidence in `dissector-state-conventions.md`, so no duplicate rule was added.
- **!21195 — high weight, merged master.** Aethra's packet counter moves from file-static storage into `wth->priv`, because the static value is wrong for later passes/files. This independently reinforces the existing per-instance parser-state/reentrancy guidance.
- **!21196 and !21199 — high weight, merged master; Michael Mann architecture refactors.** The SCSI SRT UI calls a generic SRT parameter callback instead of a dissector-specific exported function, while an AMR helper used only by RTP media UI moves out of the dissector. Both reinforce the established rule that generic UI should not link directly against individual dissector implementations when a generic interface or consumer-local implementation suffices.
- **!21227 and !21198 — high weight, merged master.** SMB SID-snooping globals are removed from a tap whose unfinished feature would require redesign, while CAMEL/TCAP SRT information is computed without sharing tap globals. These reinforce explicit ownership and reduction of hidden global/tap coupling.
- **!21201 and !21202 — high weight, merged safety fixes.** Pcapng overflow/leak hardening and pcap integer-underflow validation reinforce the notebook's existing checked-arithmetic and error-path ownership rules. !21213/!21214 also backport the C23-compatible checked-arithmetic support used by such fixes.
- **!21218 — high weight, merged HTTP lifetime fix.** Header-map allocation is aligned with the map/container lifetime, reinforcing allocator-scope consistency rather than adding a distinct new rule.
- **!21212 — merged build/sanitizer portability fix.** Marks non-returning tails so GCC ASAN builds understand control flow. Useful toolchain evidence, but no independent broad convention was promoted.
- **!21237 — merged Coverity fix.** Zero-initializes a BLF structure to prevent propagation of uninitialized fields; straightforward static-analysis hardening already covered by existing initialization guidance.

## Lower-information / maintenance clusters

- **Generator compatibility and backports:** !21240, !21239, !21238, !21236 reorder the `make-enums.py` header list for PyClibrary compatibility; !21233-!21235 are stable variants of !21228's sanity guard. These were inspected as accepted maintenance evidence but weighted below the master change.
- **Automatic data updates:** !21229-!21232 are automatic update attempts/variants. Closed attempts !21223-!21226 were down-weighted as non-accepted outcomes.
- **Protocol-specific fixes/extensions:** !21221/!21220/!21217 (TECMP remaining data), !21211 (Falco geolocation), !21208 (NMEA `!` start character), !21207 (802.11 PSK optimization), !21194/!21192 (vendor-specific 802.11 endianness), and !21191 (protobuf `editions`) were inspected but did not justify new cross-project rules.
- **Leak/cleanup maintenance:** !21209/!21205 free extcap capture-filter state; !21206/!21204 validate capture linktype before constructing argv; !21219 removes unused Falco members. These corroborate ordinary ownership/cleanup practice.
- **Checker/build hygiene:** !21210 fixes dissector warnings; !21193 adjusts `check_typed_item_calls` for one-byte unsigned fields with `ENC_NA`; !21197 re-enables Falco plugins on Windows; !21203 is a comment typo fix. No new durable rule.
- **UI feature work:** !21216 adds multiple plot value axes. Merged feature work, but no broadly reusable maintainer convention emerged from this corpus snapshot.

## Weighting notes

Merged master changes with explicit rationale were weighted highest. John Thacker's memory/state changes (!21222, !21200) and Michael Mann's dependency-boundary changes (!21215, !21196, !21199) were treated as authoritative project evidence. Gerald Combs's generator safeguard !21228 receives very high weight because he authored, approved, and merged the master change and propagated it to maintained branches. Stable backports and automatic updates were used as corroboration only. Closed !21223-!21226 were not treated as accepted implementation precedent.

## Notebook files changed in this run

- `generated-output-conventions.md`
- new `wiretap-format-detection-conventions.md`
- this ledger file

After this run, the contiguous reviewed frontier for this backward pass is below **!21191**, subject to the same exact-set reconstruction on the next run.