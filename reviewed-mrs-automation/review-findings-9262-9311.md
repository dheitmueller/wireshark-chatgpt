# Review findings: !9262-!9311

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Reviewed exactly 50 previously unreviewed MRs, !9311 through !9262: 41 merged, eight closed/unmerged, and one open/unmerged (!9279). Merged work received the strongest weight.

Key durable findings:

- !9281 and corrective !9298: preserve the protocol-defined field domain. Pascal Quantin noted that MBIM fields specified as 32-bit scalar values should not become Boolean merely because current values are 0 and 1. This was added to boolean-field-domain-conventions.md.
- !9283: Martin Mathieson's typed-item checker associates field arrays with actual bitmask call sites and detects overlapping member masks; it immediately found real LAT, NFS, and RTLS errors.
- !9272 followed by !9303: generated Flex output differed on an older supported toolchain. The accepted response was a narrow Flex-specific warning suppression, verified on RHEL 7.9/GCC 4.8.5.
- !9299 plus merged !9266/!9280: Alexis La Goutte's guidance supports landing the authoritative fix on master first, then creating explicit stable-branch backports.
- !9273: the Zstandard TVBuff fix uses one owned decompression output buffer and explicitly tests a valid zero-output result.
- !9285/!9284 corroborate the existing finite recursion-depth rule.
- Open !9279 contains high-authority Guy Harris feedback against arbitrary root-tree special-casing for selected transport payloads; because it remains unmerged, the design evidence is provisional.

!9300 also reinforces representative-capture and cross-platform build validation. !9306, authored and merged by John Thacker, is useful Python compatibility evidence. Closed !9311-!9307 and !9299 were down-weighted; !9310 is negative branch-hygiene evidence because it accumulated hundreds of unrelated changes.

The historical !17571-!17620 batch remains preserved as exactly 50 reviewed MRs.
