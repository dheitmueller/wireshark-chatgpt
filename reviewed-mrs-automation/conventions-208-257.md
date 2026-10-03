# Convention synthesis: MRs !208-!257

This batch is dominated by early GitLab migration-era fixes and stable-branch backports, but it contains several durable conventions.

1. **String field types must encode the actual wire termination/padding contract.** Guy Harris's merged master !234 introduces `FT_STRINGZTRUNC` for fixed-width fields that terminate with NUL but may contain unspecified nonzero bytes afterward. Contrast true NUL padding (!224) with explicitly non-NUL-terminated strings (!212/!221, !223). Interim !225 is superseded by !234 for SAP semantics.

2. **Do not preinitialize parser-control locals to plausible defaults when valid control flow must assign them.** Guy Harris's !222 notes that such initialization can prevent compilers/static analyzers from exposing a missing meaningful assignment. The accepted fix also uses `proto_tree_add_item_ret_uint()` so the displayed field and parser-control value are decoded once.

3. **Masks and value tables form one semantic contract.** !217 fixes both Q.933's mask and its normalized value table. Value-string keys for a masked integer field live in the post-mask/post-shift value domain, not raw packet bit positions.

4. **Separate textual sentinels from arbitrary binary payload.** Guy Harris's !255 identifies the exact LIP Echo magic string and represents later bytes as `FT_BYTES`; arbitrary binary data must not be coerced to text simply because it follows a textual signature.

5. **When a newly dissected field also drives labels or parsing, use the registered-field return API and provide a representative capture.** Alexis La Goutte's review of !238 explicitly requests `proto_tree_add_item_ret_uint()` and a pcap; both are present in the merged result.

6. **Release-branch CI can legitimately differ from master when provenance and available tooling differ.** !216/!232 disable redundant commit-message validation for GitLab-generated cherry-pick metadata and omit an impractically slow stable-branch cppcheck configuration; Guy Harris also documented GitLab's cherry-pick workflow in the submission wiki.

7. **Dependency discovery should probe the dependency's actual installed layout while preserving supported fallbacks.** !228 checks for libssh's newer `libssh_version.h` and falls back to the older `libssh.h` location instead of hard-coding one version's header layout.

8. **Review protocol extension identifiers against their authoritative provenance.** !215 distinguishes a draft QUIC timestamp parameter from vendor-specific extensions and attaches representative pcaps directly to the MR; maintainer review caught a missing draft-defined value.

No new ST 291/VANC packet types were encountered.
