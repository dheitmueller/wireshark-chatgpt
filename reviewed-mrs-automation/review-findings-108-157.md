# Review findings: Wireshark MRs !108-!157

This run reviewed the discussion and diff carried in each corpus record. Merged master changes are the strongest implementation evidence. Stable-branch backports mainly corroborate master behavior. Closed !127 is retained only for its unusually authoritative standards discussion.

| MR | Outcome / weight | Review result |
|---|---|---|
| !157 | Merged master, low | Minor spelling-tool variable cleanup and dictionary additions. |
| !156 | Merged master, medium | TLS gains missing Google QUIC transport parameters; review requested reuse of the existing gQUIC tag decoder, iteration over repeated four-byte versions, and hierarchical filter names. |
| !155 | Merged master, low | Translation-file refresh; no durable coding convention. |
| !154 | Merged stable, corroborating | AARP rejects zero hardware/protocol address lengths before address conversion and reports expert information. |
| !153 | Merged stable, corroborating | Same AARP invalid-length guard for another maintained branch. |
| !152 | Merged stable, low | Automatic registry/data update. |
| !151 | Merged stable, low | Automatic registry/data update. |
| !150 | Merged stable, low | Automatic registry/data update. |
| !149 | Merged master, low | Automatic registry/data update. |
| !148 | Merged master, medium | Introduces the spelling checker and project dictionary; establishes source/documentation spell checking as project tooling. |
| !147 | Merged master, low | Bulk documentation spelling cleanup; discussion checked suspicious whitespace rather than blindly trusting rendered diff highlighting. |
| !146 | Merged master, low | Adds SMB AES-256 cipher identifiers from the protocol specification. |
| !145 | Merged master, medium | E1AP v16.2 update keeps ASN.1 sources, templates/configuration, generated dissector output, and field-capacity adjustment in one coherent generated-code update. |
| !144 | Merged master, medium | Elasticsearch parsing becomes version-aware for variable headers/thread context/features. Review questions hiding empty-payload warnings instead of validating length. |
| !143 | Merged master, medium | F1AP v16.2 update and generated headers expose reusable generated PDUs to related dissectors. |
| !142 | Merged master, medium | Fixes NGAP generated-header provenance/comment and adds the template header to the generator input list so generated output stays reproducible. |
| !141 | Merged master, medium | IEC104 adds CP24Time2a. Anders Broman agrees a bit mask should be removed unless showing that bit materially improves dissection; reviewers also ask for a real sample capture. |
| !140 | Merged master, high | Cleans checkAPI and extends it to Qt-owned code while explicitly excluding third-party QCustomPlot sources that intentionally use prohibited APIs. Strong checker-scope precedent. |
| !139 | Merged stable, corroborating | S1AP field mask corrected from 0x1f to 0x07 in source template and generated dissector. |
| !138 | Merged stable, corroborating | Same S1AP mask fix on another release branch. |
| !137 | Merged stable, corroborating | Same S1AP mask fix on another release branch. |
| !136 | Merged stable, corroborating | X2AP field mask corrected in template and generated output. |
| !135 | Merged stable, corroborating | Same X2AP mask fix on another release branch. |
| !134 | Merged stable, corroborating | Same X2AP mask fix on another release branch. |
| !133 | Merged master, high | Master S1AP mask correction; reinforces that generated source and generator/template source must agree. |
| !132 | Merged master, high | Master X2AP mask correction; same generator/source consistency rule. |
| !131 | Merged master, medium | XnAP uses a bitmask list to expose independently meaningful flags plus reserved bits from one 32-bit value. |
| !130 | Merged master, historical | Spelling cleanup also renamed a misspelled display-filter abbreviation. Later stronger notebook evidence warns that filter abbreviations are compatibility identifiers, so do not generalize this old rename as current policy. |
| !129 | Merged master, low | GTP NR RAN extension updated to newer specification values and names. |
| !128 | Merged master, high | GitLab issue templates request reproduction steps, sample captures for dissector bugs/enhancements, logs, and build info. Gerald also requires new repository paths to be covered by the license checker and asks for a single squashed logical commit when he cannot rebase the branch. |
| !127 | Closed/unmerged, high-authority discussion | Proposed TACACS+ Accounting Reply `data` conversion from string to bytes was rejected after Guy Harris and Chuck Craft traced the normative drafts: this field is defined as printable text, while only specifically identified protocol-processing data fields are arbitrary octets. |
| !126 | Merged stable, corroborating | TACACS accounting reply parser fixes the query/reply offset constant on a stable branch. |
| !125 | Merged stable, corroborating | Same TACACS offset fix on another stable branch. |
| !124 | Merged stable, corroborating | Same TACACS offset fix on another stable branch. |
| !123 | Merged master, very high | Guy Harris and Peter Wu establish that QUIC streams, like TCP, are ordered byte streams without application-message boundaries. QUIC subdissectors may request reassembly through `pinfo->desegment_offset` / `desegment_len`; existing NBSS/SMB framing should be reused instead of assuming one QUIC STREAM frame equals one SMB PDU. |
| !122 | Merged master, medium | Extends practical support for Q050/T050/T051 hybrid Google QUIC traffic and asks for representative pcaps. |
| !121 | Merged master, high | RTP always registers the raw `rtp.payload` field, then hides it when a payload subdissector successfully decodes the data. Raw bytes remain filterable/available without cluttering the tree when semantic dissection succeeds. |
| !120 | Merged master, process | Reiterates the requirement to allow maintainer commits so core developers can rebase MRs instead of repeatedly bouncing conflicts back to contributors. |
| !119 | Merged master, high process | Tested TACACS fix. Pascal Quantin explicitly requires the protocol/component name at the beginning of the commit subject and points to the submission guide. |
| !118 | Merged master, high | Updates portability guidance: source may be UTF-8, but non-ASCII should be used sparingly because older compilers, editors, and consoles differ; Qt strings are UTF-16 while most Wireshark strings and console output are UTF-8. |
| !117 | Merged master, process | CI makes the "allow maintainer commits" requirement deliberately prominent because it is operationally important to review/rebase flow. |
| !116 | Merged master, low | Documentation spelling fixes. |
| !115 | Merged master, low | Man-page spelling fixes. |
| !114 | Merged stable, corroborating | Stable backport of GTPv2 Target Identification fix; discussion reflects project-controlled cherry-pick/backport workflow. |
| !113 | Merged master, medium | GTPv2 refactors an ID decoder to accept the specific `hfindex` and uses `proto_tree_add_item_ret_uint()` so the displayed field and parser value share one registered decode. |
| !112 | Merged master, medium | Diameter Codec-Data parses bounded lines with `tvb_find_line_end()`, checks the returned next offset, and then delegates the SDP portion; useful layered text-parsing example. |
| !111 | Merged master, very high | Adds documentation CI. Peter Wu requests reuse of the existing development image instead of rebuilding an environment; the job is scoped to documentation/WSLua changes and publishes generated manuals as review artifacts. Gerald gives explicit interactive-rebase guidance to turn a long fixup history into one logical commit. |
| !110 | Merged master, process | Documentation contribution; Pascal again asks the contributor not to accumulate review-fix commits and to update the MR as one logical change. |
| !109 | Merged master, medium | XnAP v16.2 generated-code update, including exported helper PDUs from related ASN.1 dissectors. |
| !108 | Merged master, process | Developer guide documents enabling maintainer commits and optionally deleting the source branch after merge. |

## Highest-value synthesis

1. **Stream transports do not create application PDU boundaries.** Guy Harris's !123 review makes this explicit for QUIC and TCP. The subdissector should use the transport's standard desegmentation contract and preserve its own framing layer.
2. **Field type follows the normative semantic contract.** Closed !127 is strong negative evidence: a field specified as printable text should not become `FT_BYTES` because one implementation calls a related container "octets."
3. **Raw payload and semantic dissection can coexist.** !121 preserves `rtp.payload` as a registered field but hides it when a successful subdissector provides a better visible interpretation.
4. **CI should produce reviewer-useful artifacts and reuse project infrastructure.** !111 uses the standard dev image, path-scoped rules, and generated-manual artifacts; !140 expands static checks to first-party Qt code while explicitly excluding vendored sources.
5. **Generated-code changes must keep source-of-generation and generated output synchronized.** !133/!132, !142, !145, !143, and !109 all reinforce updating templates/configuration and checked-in generated files together.
6. **Source encoding is UTF-8, but portability still matters.** !118 allows UTF-8 while recommending restraint for non-ASCII because toolchains, editors, and consoles remain heterogeneous.
7. **Submission history should describe one logical change.** !128, !111, !110, !120, !117, !119, and !108 reinforce squashing review fixups, component-prefixed subjects, and allowing maintainer rebases/minor edits.
