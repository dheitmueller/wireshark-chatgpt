# Convention synthesis — !2911–!2960

- **Embedded child dissection (!2947):** create the child tvbuff/data source at the semantic boundary and contain nested exceptions when a malformed embedded structure should not abort the parent. Separate stateful and state-neutral child entry points when caller context differs.
- **Vendor filter namespaces (!2945, Anders Broman):** put enterprise-specific fields below both the protocol and enterprise name, e.g. `pfcp.bbf.*`. Avoid flat vendor fields and stale names copied from another protocol.
- **Scheduled fuzzing (!2923, !2934, !2938, !2942, !2943, !2956, !2957, !2960; Gerald Combs):** retain the failing capture and stderr/log evidence, exercise complementary engines, and use resource groups that prevent conflicting runs without unnecessarily serializing unrelated branches.
- **Submission metadata (!2926, Pascal Quantin):** GitLab submissions do not need the old Gerrit `Change-Id` trailer; install/update Wireshark's hooks from `tools/` rather than carrying Gerrit-era metadata forward.
- **Post-merge corrections (!2915 → !2927, Pascal Quantin):** once an MR has merged, later correctness fixes get a new MR. The merged MR is immutable review history, not a branch to extend.
- **Helper reuse and review vectors (!2928):** prefer an existing tvbuff/proto helper over a local duplicate when it represents the same wire encoding, and provide a focused pcap plus release-note entry for a new protocol.
- **Generated ASN.1 parity (!2926, !2946):** update authoritative ASN.1/conformance/template inputs and commit the regenerated dissector output together.
- **Build-history weighting (!2944 vs. later !3004/!3008):** an earlier merged workaround is useful historical evidence, but later accepted corrective work should remain the stronger current convention.

No SMPTE ST 291/VANC packet type was encountered.
