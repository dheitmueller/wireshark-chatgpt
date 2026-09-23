# Automated MR review ledger: !12312 through !12263

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Selection method: rebuilt the already-reviewed set from the notebook review tracking, including `reviewed-mrs.md` and the per-run files under `reviewed-mrs-automation/`, preserving the historical !17571-!17620 batch. The fifty highest-numbered corpus MRs not in that set were selected individually; the resulting set happens to be contiguous. The earlier !12312 lookup was only a frontier probe and was not treated as a completed review.

## Exact reviewed MR set

!12312, !12311, !12310, !12309, !12308, !12307, !12306, !12305, !12304, !12303,
!12302, !12301, !12300, !12299, !12298, !12297, !12296, !12295, !12294, !12293,
!12292, !12291, !12290, !12289, !12288, !12287, !12286, !12285, !12284, !12283,
!12282, !12281, !12280, !12279, !12278, !12277, !12276, !12275, !12274, !12273,
!12272, !12271, !12270, !12269, !12268, !12267, !12266, !12265, !12264, !12263.

Count: **50**.

## Review notes and durable findings

- !12312 / !12295 — NTP removes an event value that is outside the field mask and absent from RFC 9327. Merged master plus release backport; reinforces validating value tables against both the registered mask and the authoritative specification.
- !12311 / !12309 — pcapng Sysdig meta-event support centralizes block identifiers through `pcapng_module.h` rather than maintaining a second local set in the dissector. Merged behavior; good corroboration for keeping shared format constants in their owning wiretap/module layer.
- !12310 / !12308 — Qt protocol-byte highlighting fix. The accepted master change reconstructs parent-field context when a mouse click reselects an already-selected item and parents the temporary `FieldInformation` object to the `ProtoTree`; useful evidence that GUI event paths that appear equivalent can carry different context/lifetime semantics.
- !12307 — Debian symbol maintenance. Packaging-only; no new durable rule promoted.
- !12306 — XML bounds a regex capture group. Merged security/robustness hardening; reinforces bounding regex work when input comes from packets rather than relying on an otherwise-valid expression to have acceptable resource use.
- !12304 (and related backport material in the batch) — fixes a duplicate/conflicting RTP event identifier in a value table. Reinforces checker-driven validation of value tables; no separate rule added.
- !12300 — release-note cleanup; editorial/release maintenance only.
- !12299 / !12275 — Lemon accepts `--` as the end of options and Wireshark uses it when invoking Lemon, preventing source/build paths beginning with option-like characters from being parsed as switches. Strong corroboration for treating paths as data across tool boundaries.
- !12298 / !12293 — Qt/Logray titlebar slot corrections; localized UI fixes.
- !12297 — adds a MinGW build to merge-request CI. Useful evidence that portability-sensitive code paths should be exercised in MR CI rather than only scheduled/release jobs.
- !12296 / !12286 — JSON boolean representation correction/revert family. Accepted direction preserves native JSON boolean values instead of stringifying them; existing representation/serialization guidance is sufficient.
- !12294 — fixes a compile error caused by an API call not being updated for a new parameter; straightforward API migration cleanup.
- !12291 — replaces a `FILENAME_MAX+12` stack array used to form `printname:<path>` with dynamically sized `g_strdup_printf()` storage, then frees it after the argument helper has copied it. Merged and approved by John Thacker. Promoted to `path-and-buffer-conventions.md`.
- !12289 and !12287 — closed RF4CE dissector submissions. These were down-weighted because they were not merged; they are useful only as discussion/submission evidence, not accepted implementation precedent.
- !12288 / !12272 / !12271 / !12270 — naming cleanups for BCD digit sets correct misleading `1-9` terminology when the actual domain is `0-9`. Useful naming/semantic precision; no separate notebook rule required.
- !12285 — CI build-directory naming fix tied to ccache behavior; CI maintenance only.
- !12284 / !12278 — documentation/spelling cleanup; no durable code rule.
- !12283 / !12282 / !12281 / !12277 — ring-buffer minimum-file-count logic correction and backport family. Reinforces testing configurable bounds at non-default values; no new notebook file needed.
- !12280 — exposes dumpcap's `-b printname:FILE` capability through TShark. The later merged !12291 refines its temporary-string implementation.
- !12276 — Debian symbol/override version maintenance; packaging-specific.
- !12274 — changes the shared ring-buffer option parser to fail when the inner `-b name:value` name is unknown, aligning TShark/dumpcap with `-a`. Promoted to `cli-option-semantics-conventions.md`.
- !12269 — iperf3 UDP loss/out-of-order rework and release backport material; stateful sequence-accounting fix, retained as corroboration rather than a new general rule.
- !12266 — Gerald Combs's merged GitLab CI+CMake change quotes a path used in a wrapper script and deliberately builds in a directory containing spaces and emoji. Promoted to `path-and-buffer-conventions.md`: arbitrary valid build paths should be preserved through command boundaries and awkward path names should be exercised continuously in CI.
- !12265 — WSDG automated-build documentation update; documentation-specific.
- !12264 / !12263 — version/release-4.2 branch initialization bookkeeping; no reusable coding rule.

Other MRs in the exact set were inspected for state, discussion, and diff context but did not add a durable convention beyond existing notebook material. Merged changes were treated as stronger evidence than abandoned/superseded work; closed !12289 and !12287 were explicitly down-weighted. No substantive Guy Harris review in this batch was used to manufacture a rule; the strongest accepted-authority evidence promoted here comes from Gerald Combs (!12266), John Thacker's approval/merge of !12291, and the merged behavior itself.

## Notebook updates from this run

- `cli-option-semantics-conventions.md`: structured CLI options must reject unknown inner names rather than silently accepting/ignoring them (!12274).
- `path-and-buffer-conventions.md`: avoid nominal path-limit macros for derived strings (!12291), and continuously exercise build/tool paths containing whitespace and non-ASCII characters (!12266; also corroborated by !12299/!12275).

This ledger is the authoritative per-run record for these fifty MR numbers.