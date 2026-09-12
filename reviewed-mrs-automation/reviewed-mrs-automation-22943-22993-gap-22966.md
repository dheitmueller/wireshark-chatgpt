# Wireshark MR review automation ledger — !22993 back through !22943

Corpus: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Direction: newest available previously-unreviewed MRs toward older MRs.

This run reviewed exactly **50** previously unreviewed merge requests. Selection was built from individual MR entries across `reviewed-mrs.md`, the aggregate automation ledger, and the per-run files under `reviewed-mrs-automation/`; numeric ranges were not assumed complete. The historical !17571-!17620 review batch remains part of the already-reviewed set and was not revisited.

!22994 was already recorded in the immediately preceding backfill run. The corpus contains no `mr_22966.json`, so the exact next 50 are !22993 through !22967 and !22965 through !22943.

## Exact MRs reviewed

`!22993 !22992 !22991 !22990 !22989 !22988 !22987 !22986 !22985 !22984 !22983 !22982 !22981 !22980 !22979 !22978 !22977 !22976 !22975 !22974 !22973 !22972 !22971 !22970 !22969 !22968 !22967 !22965 !22964 !22963 !22962 !22961 !22960 !22959 !22958 !22957 !22956 !22955 !22954 !22953 !22952 !22951 !22950 !22949 !22948 !22947 !22946 !22945 !22944 !22943`

Count: **50**.

## High-value findings

- **!22943 — deep, merged.** Michael Mann moved generic file-error values into `wsutil` so the lower utility layer no longer depends on `wtap.h` merely to borrow Wiretap errors. Guy Harris approved and merged the change. Promoted as very-high-confidence dependency-direction guidance in `application-layer-boundary-conventions.md`.
- **!22947 — deep, merged.** John Thacker fixed P4RPC undefined behavior by invoking UTF-8 tail repair only when bounded formatting actually reaches the destination end. The old path could inspect uninitialized bytes. Promoted to `string-api-conventions.md` as a general rule to keep formatter return semantics, initialized extent, capacity, and NUL accounting distinct.
- **!22950 — deep, merged.** Jaap Keuter standardized uncertain-length dissectors to begin with a conservative protocol-item span and later reduce it; the MR explicitly says analysis engines such as MATE depend on that behavior. Michael Mann merged it. Promoted to `proto-tree-range-conventions.md` as a shrink-only range-refinement rule. Anders Broman's related unsigned-offset suggestion was deliberately deferred to a separate broad migration, also reinforcing focused-change scope discipline.
- **!22956 — architecture-focused, merged.** Michael Mann generated distinct registration/handoff entry points for event dissectors used by Stratoshark and removed the previous `epan_supports_packets()` mechanism. This reinforces the already-recorded frontend-registration direction: encode real frontend capabilities/registration families explicitly rather than spreading runtime product probes. No duplicate notebook rule added.
- **!22969 — deep, merged.** Gerald Combs changed `make-dmx-manfid.py` to construct output before writing it so an exception cannot leave a partial generated file. Promoted to `generated-output-conventions.md` as transactional generator-output guidance.
- **!22974 — merged security fix.** Adds a missing SMB2 state/file validity check and was approved and merged by John Thacker. Useful defensive evidence, but existing state/precondition guidance already covers the general lesson.
- **!22975 — merged.** John Thacker corrected lazy Qt initialization so hover-highlight state is read when the widget is created rather than waiting for a context menu that is now constructed on demand. Good GUI lifecycle evidence; treated as corroborating rather than broad enough for a new notebook rule.
- **!22987 and !22991 — deep architecture, merged.** Michael Mann moved product-specific extcap-directory and VCS-version selection behind application-layer APIs. !22991 deliberately keeps `dumpcap` independent instead of adding an upward application-layer dependency merely for reuse. Promoted together to `application-layer-boundary-conventions.md`.
- **!22992 — discussion-focused, merged.** During RESP3 review, John Thacker recommended splitting the particularly tricky, largely independent Map dissection into a separate MR to accelerate review, and caught a type/API mismatch (`proto_tree_add_string` on an `FT_BOOLEAN`). This strongly corroborates focused submissions and field-type-correct protocol-tree APIs; existing notebook guidance already covers those themes.

## Corroborating clusters

- **TVBuff/raw-pointer and bounded-search safety:** !22985 and !22984 avoid unbounded `tvb_get_ptr(..., -1)` patterns whose returned pointer was later searched with reported length; !22983/!22982 add/use remaining-length find APIs; !22976/!22970/!22960/!22953 use subset TVBs to constrain descriptor parsing. These reinforce existing TVBuff captured-length, subset-boundary, and remaining-length guidance rather than creating duplicates.
- **Unsigned TVBuff API migration:** !22967, !22946, and !22944 continue the accepted unsigned-offset cleanup. The notebook already records the numeric-domain rationale, so no duplicate rule was added.
- **Truncation/precondition handling:** !22954 explicitly checks bytes before calling a downstream data dissector that does not itself throw for missing data. This reinforces the rule that each entry path must enforce the preconditions it actually requires.
- **Static-analysis and cleanup fixes:** the DCT2000, Thrift, MEGACO, WPS, DHCP, and related fixes in this batch mostly repair concrete Coverity/compiler findings and corroborate existing null-check, return-status, allocator, and parser-boundary guidance.
- **Frontend/product separation:** !22949 and the !22956 registration work support the same architectural trajectory as !22987/!22991 and the previously reviewed !22994/!23006 GUI refactors.
- **Maintenance/automation/backports:** automatic data updates, release/stable variants, CI/package adjustments, documentation, and small protocol extensions were inspected but did not independently justify new durable notebook rules.

## Weighting and non-promotions

Merged master changes with explicit rationale and maintainer approval were weighted most heavily. !22943 receives especially high confidence because Guy Harris approved and merged the layering cleanup. John Thacker's authored/merged !22947 and substantive !22992 review comments were also treated as authoritative.

Open **!22990** was treated as provisional and was not used to establish accepted architecture. Repeated/backport/superseding variants in the Thrift, DCT2000, DHCP, Qt, and automated-update sequences were used only as corroboration unless the merged master change added independent rationale.

## Notebook changes from this run

- `application-layer-boundary-conventions.md`: application-specific policy belongs behind the application layer; reusable low-level layers must not depend upward merely to borrow error definitions.
- `generated-output-conventions.md`: build/validate fallible generated output before publishing it so failures cannot leave partial files.
- `proto-tree-range-conventions.md`: uncertain protocol-item ranges should start conservatively broad and shrink once exact bounds are known.
- `string-api-conventions.md`: use bounded-formatter return semantics before touching a possibly truncated UTF-8 tail; do not confuse capacity with initialized string extent.
