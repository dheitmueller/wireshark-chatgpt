# Review findings: MRs !208-!257

## Highest-value findings

### !234 — Add `FT_STRINGZTRUNC` (merged master, Guy Harris)

This is the strongest core-API result in the batch. It distinguishes null-padded fixed-width strings from null-truncated fixed-width strings whose bytes after the terminator are unspecified. The change propagates the new type through field registration, display-filter compatibility, protocol-tree decoding, WSLua, Decode As/UI models, checker handling, and documentation. It also replaces explicit lists with `IS_FT_STRING()` where behavior applies to the whole string family.

The surrounding MRs strengthen the rule: !224 is genuine NUL padding; !212/!221 and !223 are fixed strings that are not NUL-terminated; !213 records an observed “NUL then nonzero cruft” case. Stable !225 is an interim SAP treatment that is superseded by !234's master semantics.

### !222 — NCP syntax value/dataflow correction (merged master, Guy Harris)

The code displayed an NDS syntax field but failed to fetch its value, then passed an unrelated local initialized to zero to `print_nds_values()`. Guy's fix declares the value without a misleading default and obtains it with `proto_tree_add_item_ret_uint()`. His commit message explicitly warns that preemptive local initialization can conceal missing meaningful assignments from compiler/static-analysis dataflow checks. !226, !253, and !254 are stable backports.

### !217 — Q.933 PVC Status field semantics (merged master)

Harald Welte's fix, reviewed by Pascal Quantin, corrects both the status mask and its value table. The old table used values shifted into raw packet positions even though the registered mask normalizes the field before value lookup. The final table uses logical post-mask values and the mask includes the omitted Active bit. !218-!220 are release backports. Pascal's squash discussion is useful process evidence: tightly related fixes to one field can reasonably be one backport unit when that materially simplifies stable maintenance.

### !255 — NCP Large Internet Packet Echo framing (merged stable, Guy Harris)

The previous code treated too much of a LIP Echo packet as ASCII and conflated two packet forms. The accepted fix proves enough bytes exist before matching the complete magic, separates the textual marker (`FT_STRING`) from arbitrary binary echo payload (`FT_BYTES`), and avoids ordinary NCP connection/task parsing for the special framing.

### !238 — IEEE 802.11 Beacon Timing element (merged master)

Alexis La Goutte requested `proto_tree_add_item_ret_uint()` for values needed after tree insertion, caught naming/filter-abbreviation issues, and requested a pcap. The merged code follows that direction and the contributor attached `beacon_time.pcap`.

### !216 / !232 — release-branch CI and cherry-pick provenance

These merged release-branch MRs remove `validate-commit.py` and the old cppcheck invocation from stable CI. Their rationale is branch-specific: stable changes are normally cherry-picked from already-validated master commits; GitLab-generated cherry-pick metadata could trip whitespace validation; and the release branches lacked cppcheck efficiency work. Guy Harris asked about GitLab's cherry-pick UI and then documented it in Wireshark's SubmittingPatches wiki.

### !228 — libssh version-header layout detection (merged master)

libssh 0.9.5 moved version macros from `libssh.h` into `libssh_version.h`. Wireshark checks whether the newer header exists and falls back to the legacy location. !229-!231 carry the correction to maintained releases.

### !215 — QUIC transport-parameter provenance (merged master)

The patch adds draft/vendor QUIC transport parameters and two captures. Alexis La Goutte accepted pcaps attached directly to the MR, distinguished timestamp-v2 draft semantics from vendor-specific interpretation, found the draft-defined value 3, and requested ordering cleanup. Standards-draft and vendor-extension identifiers should retain separate provenance.

## Additional reviewed material

- !257 removes an unused NCP local on a stable branch; no new convention.
- !256, !249, !248 are spelling-only maintenance.
- !254/!253 are stable backports of !222.
- !252 and !250 are indentation-only cleanup. Closed !251 is superseded by !252 and is not implementation precedent.
- !247 adds GQUIC Q046; Alexis requested a named/commented numeric magic so its ASCII meaning is apparent and the MR references captures.
- !246-!244 are automatic data/release updates. !243 also corroborates the maintainer-edit submission practice.
- !242-!240 are stable backports of 64-bit `BASE_SPECIAL_VALS` display behavior.
- !239 is broad spelling/filter-name cleanup.
- !237 is a stable documentation correction; !236 is Qt packet-diagram label placement.
- !235/!233 are stable backports of !227.
- !227 fixes PFCP C-TAG/S-TAG semantics and explicitly records an interpretation where specification wording is ambiguous.
- !225 is superseded string-type evidence; !224, !223, !221, !213, !212 form the string-termination sequence summarized above.
- !214 is a stable NAS-5GS PDU-type correction.
- !211 updates QUIC to draft-30; review catches the companion invariants draft version, reinforcing coherent version-reference updates.
- !210-!208 are stable backports of a GSM spare-bit position fix.

No SMPTE ST 291/VANC packet type was encountered.
