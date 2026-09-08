# Supplemental Reviewed Wireshark Merge Requests — !26058–!26077

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit used for this batch: `2261458afc3e287c111ffe13f534d9aaf5af07af`

Exactly twenty MRs were reviewed in this batch: **!26058 through !26077 inclusive**.

| MR | State / target | Review depth | Durable result / weighting |
|---|---|---|---|
| !26058 | merged, master | Medium | AppleTalk short-DDP hidden src/dst fields corrected to the actual one-byte source/destination locations. Good byte-ownership correctness example; no new rule beyond existing field-location guidance. John Thacker merged/approved. |
| !26059 | merged, master | Medium | Pascal Quantin NAS 5GS SOR fix gates an optional PLMN/access-technology list on its list-indication bit before consuming its length/body. Reinforces presence-bit-before-optional-body parsing; not promoted separately. |
| !26060 | merged, release-4.6 | Backport/corroboration | Backport of !26059. Corroboration only, not independent evidence. |
| !26061 | merged, master | Deep | Daintree textual timestamp fraction is parsed as bounded decimal digits, right-padded to nanosecond precision, then converted. Promoted to `parser-edge-cases.md`: variable-width decimal fractions must preserve decimal-place semantics rather than be treated as a fixed-unit integer. |
| !26062 | merged, master | Deep | John Thacker CMake change links object libraries as targets and places libssh usage requirements on `ssh-base` with PUBLIC scope rather than manually injecting object files and duplicating dependencies. Promoted to `build-conventions.md`. |
| !26063 | merged, master | Medium | John Thacker Qt fix guards RTP-analysis derived declarations/definitions when Qt Multimedia removes the corresponding base API. Strong optional-feature build precedent; retained in ledger. |
| !26064 | merged, master | Medium | Follow-up to !26063 guards the remaining caller as well. Reinforces that feature-conditionals must cover the whole declaration/definition/call surface, not merely one layer. |
| !26065 | merged, master | Deep | John Thacker IPv4 heuristic accepts a reported-length mismatch such as an Ethernet trailer when enabled checksum validation provides stronger independent evidence that the IPv4 header is genuine. Strong heuristic-design precedent: combine structural signals rather than treating one nonessential mismatch as absolute. Existing heuristic notebook guidance already covers conservative recognition, so retained here without duplicate prose. |
| !26066 | merged, master | Scanned | asn2wrs Python f-string quoting cleanup. Useful compatibility/readability maintenance but no durable Wireshark-specific convention promoted. |
| !26067 | merged, master | Medium | Pascal Quantin NAS 5GS copy/paste fix uses each nested container's parsed `cont_len` for both tree length and cursor advance instead of the outer `len`. Reinforces local semantic length ownership already captured elsewhere. |
| !26068 | merged, release-4.4 | Backport/corroboration | Backport of !26059. Corroboration only. |
| !26069 | merged, release-4.6 | Backport/corroboration | Backport of !26067. Corroboration only. |
| !26070 | merged, release-4.4 | Backport/corroboration | Backport of !26067. Corroboration only. |
| !26071 | merged, master | Medium | Stratoshark default event-name column changed from obsolete `sysdig.event_name` semantics to `evt.type`; description notes cached user profiles can mask changed defaults. Product/default-migration example, but no sufficiently general convention promoted. |
| !26072 | merged, master | Deep | Ronnie Sahlberg UMTS RRC memory-safety fix validates both packet-derived indices against the concrete two-dimensional `seq_no` array with `G_N_ELEMENTS()` immediately before writing. Promoted to `parser-edge-cases.md`. |
| !26073 | merged, master | Discussion-focused | New IEEE 802.11 TIM Broadcast Request dissector. The contributor supplied a representative pcap and screenshot specifically to aid review before Jaap Keuter approval. Strong corroboration of the notebook's already-established sample-capture expectation; no duplicate rule added. |
| !26074 | merged, master | Medium | John Thacker extcap parser cleanup frees the owned `prefix` member alongside the rest of the argument object's strings. Straightforward ownership-symmetry fix; no new project-specific rule promoted. |
| !26075 | closed, master | Scanned | First IDN audio/StageFeed iteration closed quickly with failed pipeline and no substantive review. Down-weighted; not accepted implementation evidence. |
| !26076 | closed, master | Discussion-focused | Superseding IDN iteration also closed unmerged with unresolved discussion. Martin Mathieson explicitly pointed the author to warnings in the CI job, including spelling diagnostics; useful corroboration that CI warnings deserve attention even when the pipeline can later pass, but unmerged implementation is down-weighted. |
| !26077 | merged, master | Medium | Martin Mathieson O-RAN FH CUS change retains the `rb` proto item and attaches expert info when later section-extension context proves the combination invalid. Good contextual expert-info pattern; existing expert/malformed-data guidance is sufficient. |

## Weighting notes

Merged master-branch changes were weighted most heavily. !26060 and !26068–!26070 are release backports of master changes and were treated as corroboration rather than independent evidence. Closed !26075 and !26076 were explicitly down-weighted; their implementations were not treated as accepted architecture. !26073's sample-capture evidence and !26076's CI-warning review comment are retained as review/process corroboration.

No Guy Harris-authored or Guy Harris-reviewed MR in this twenty-MR batch supplied direct guidance. John Thacker's authored/merged !26062, !26063, !26064, !26065, and !26074 therefore receive particularly strong maintainer weight, with merged work from Pascal Quantin, Anders Broman-reviewed changes, Ronnie Sahlberg, Jaap Keuter, and Martin Mathieson weighted according to merge status and review context.

## Notebook changes from this batch

- `parser-edge-cases.md`: variable-width textual fractional-second parsing; concrete multidimensional array bounds before packet-derived state writes.
- `build-conventions.md`: CMake object libraries as link targets with usage requirements attached to the owning target.
- Existing sample-capture, heuristic-recognition, length-ownership, expert-info, and CI-review guidance was corroborated but not duplicated.

Notebook commits created during this review:

- `61fc73bc6040b75174490a1f8d9b1a3cecab9c1c` — parser edge-case conventions.
- `0531df5b2098447a089d15ff8ce5f24f390399a7` — CMake/build-system convention.
