# Review findings: !3911–!3960

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed MRs were reviewed, descending from !3960 through !3911 after exact-membership reconciliation. Merged work is weighted above closed/superseded work; maintainer-authored/reviewed changes, especially Guy Harris guidance, receive higher weight.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !3960 | merged | Scan | Clang dead-store cleanup in WoWW. |
| !3959 | merged | Deep / Guy | Fixes plugin classification logic in check_tfs. |
| !3958 | merged | Deep / Guy | Checker must not require plugins to reuse libwireshark data symbols they cannot import. |
| !3957 | closed | Down-weighted | Superseded Gryphon cleanup blocked by the checker issue fixed in !3958/!3959. |
| !3956 | merged | Deep / Guy | Type/count-specific safe multiplication avoids meaningless signed tests on sizeof and retains overflow checking. |
| !3955 | merged | Medium | macOS Arm signing/notarization CI enablement. |
| !3954 | merged | Deep | Plugin-wide conversion from ambient packet scope to pinfo->pool. |
| !3953 | merged | Scan | FAQ policy/documentation update. |
| !3952 | merged | Scan | macOS GLib dependency version update. |
| !3951 | merged | Discussion | Pascal Quantin favors a common helper over a macro and keeps private declarations in the C file. |
| !3950 | merged | Medium | Adds PFCP Session Change Info decoder after !3926 corrected its IE number. |
| !3949 | merged | Medium | Extends JSON 3GPP EPS IE decoding. |
| !3948 | merged | Deep / Guy | Meaningful no-crypto digest stub exists only at the semantic capability boundary. |
| !3947 | merged | Deep / Guy | Avoid silent no-op field-format callbacks for optional-feature builds. |
| !3946 | merged | Discussion | Warning cleanup reconciled with the actual libgcrypt feature matrix. |
| !3945 | merged | Deep | GLib is restored as a PUBLIC epan dependency because external plugins need its transitive interface. |
| !3944 | merged | Deep | H.248 explicitly threads pinfo/allocator ownership through helpers and templates. |
| !3943 | merged | Deep | ASN.1 templates migrate from ambient packet scope to explicit pinfo->pool ownership. |
| !3942 | merged | Deep | BLF correctly declares multiple packet blocks. |
| !3941 | merged | Scan | 3.2.16 release preparation. |
| !3940 | merged | Scan | 3.4.8 release preparation. |
| !3939 | merged | Medium | O-RAN ext11 extent follows actual IQ width. |
| !3938 | merged | Medium | RTPS bitmap size uses ceiling number of 32-bit words. |
| !3937 | merged | Medium | O-RAN Section Type 5 field-presence correction. |
| !3936 | closed | Down-weighted | Unmerged field-reference API; review records ABI symbol-manifest requirement for new exports. |
| !3935 | merged | Discussion | RTPS WAN locator fix; submission was folded to one commit. |
| !3934 | merged | Scan | Corrects 3GPP RADIUS dictionary entries. |
| !3933 | merged | Discussion | Pascal enforces style checks and cleaned single-commit history. |
| !3932 | merged | Deep | Adds typed-item label warning for trailing colons on non-FT_NONE fields. |
| !3931 | merged | Scan | Capture-file API const-correctness. |
| !3930 | merged | Scan | BLF FlexRay Status 2 value-table correction. |
| !3929 | merged | Medium | O-RAN UE ID uses correct return value, mask, and display. |
| !3928 | merged | Deep | DoIP/ISO15765 pass source/target diagnostic context explicitly to UDS. |
| !3927 | merged | Scan | Packet-provider const-correctness. |
| !3926 | merged | Medium | Fixes PFCP IE-number conflict and leaves decoder addition to focused follow-up !3950. |
| !3925 | merged | Medium | O-RAN block-floating-point beamforming-weight decompression. |
| !3924 | merged | Scan | Automatic release-3.2 data update. |
| !3923 | merged | Scan | Automatic release-3.4 data update. |
| !3922 | merged | Scan | Automatic master data update. |
| !3921 | merged | Discussion | Jaap Keuter prefers decimal display where hexadecimal adds no semantic value. |
| !3920 | merged | Medium | Kerberos allocator-aware helper gets packet context. |
| !3919 | merged | Deep / Guy | Exported PDU contract corrected for alignment, deprecated optional length TLV, padded strings, and byte order. |
| !3918 | merged | Medium | BLF LIN record support. |
| !3917 | merged | Deep / Guy | BLF lazily creates interface blocks; Guy clarifies block multiplicity and dynamic interface discovery. |
| !3916 | merged | Deep / Guy | Text import uses canonical Exported PDU tag definition rather than a hard-coded value. |
| !3915 | merged | Deep | RTP resampling widens before multiplication to avoid 32-bit overflow. |
| !3914 | merged | Deep / Guy | androiddump uses canonical Exported PDU tag definition rather than a duplicate constant. |
| !3913 | merged | Medium | Infiniband AETH NAK error reporting. |
| !3912 | merged | Scan | UDPCP subtree gets its real two-byte source extent. |
| !3911 | merged | Medium | LPPe updates authoritative ASN.1/config/template inputs for the newer OMA spec. |

## High-value synthesis

- **!3958 and !3959:** source checkers must understand plugin linkage and repository-path semantics; rules valid for core dissectors can be wrong for plugins.
- **!3956:** allocation arithmetic helpers should encode the real type-size/count contract and validate the actual allocator-size domain.
- **!3947 and !3948:** optional-feature guards belong at semantic capability boundaries; do not satisfy callback APIs with silent nonfunctional implementations.
- **!3945:** a CMake target's transitive include/link requirements are part of the external plugin/SDK contract.
- **!3954, !3944, and !3943:** broad early evidence for explicit `pinfo->pool` or caller-supplied allocator ownership.
- **!3917 and !3942:** Wiretap readers may discover interfaces lazily, and supported block multiplicity/options must match the format's real semantics.
- **!3919, !3916, and !3914:** shared wire-format behavior and constants need one canonical contract/source of truth.
- **!3928:** pass carrier-specific interpretation context to a child dissector explicitly rather than hiding it in global state.
- **!3932:** field labels should not bake in rendering punctuation; the checker can enforce that mechanically.
- **!3915:** widen operands before multiplication; widening after an overflow is too late.

No VANC packet type or SMPTE 291 payload family appeared in this batch.
