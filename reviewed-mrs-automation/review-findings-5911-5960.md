# Review findings: !5911–!5960

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are treated as stronger implementation evidence than closed/superseded submissions. Maintainer-authored changes and direct review from maintainers such as Guy Harris, Gerald Combs, John Thacker, João Valverde, Jaap Keuter, Stig Bjørlykke, and others are weighted according to the specificity and acceptance of the feedback.

| MR | Outcome | Depth | Review result |
|---|---|---|---|
| !5960 | merged | Scanned | John Thacker's GTP IE decoding update; protocol-specific and no new cross-cutting convention. |
| !5959 | merged | Checker-focused | Extends typed-item call matching across several source lines; corroborates robust source-checker parsing. |
| !5958 | merged | Deep | Gerald Combs restores a stable-branch exported logging symbol as a deprecated wrapper; Balint Reczey requires truthful 3.6.2 symbol provenance. Promoted to ABI notes. |
| !5957 | merged | Scanned | Adds Fedora RPM clang-toolchain support; useful build coverage but no new general rule. |
| !5956 | merged | Scanned | Zero-initializes 802.11 local buffers after Valgrind uninitialized-read findings; ordinary defensive initialization. |
| !5955 | merged | Deep | John Thacker initializes GLib temp-file output to NULL and avoids using it after a failed create; corroborates output/error-path contracts. |
| !5954 | merged | Deep | John Thacker fixes CI ancestry from `HEAD^N` to `HEAD~N`; promoted to commit-range notes. |
| !5953 | merged | Checker-focused | Expands typed-item checker to multiline calls and positive-length requirements; corroborates checker tooling. |
| !5952 | merged | Backport | release-3.4 backport of !5944; same opaque-context fix. |
| !5951 | merged | Backport | release-3.6 backport of !5944; same opaque-context fix. |
| !5950 | merged | Deep | John Thacker separates explicit RTCP/SRTCP Decode As semantics from ambiguous heuristic choice and adds a preference; Anders Broman supports the preference. Promoted. |
| !5949 | merged | Scanned | NSIS now installs/executes the exact Visual C++ redistributable filename discovered by CMake; packaging correctness. |
| !5948 | merged | Deep | SRTCP without setup metadata reports encrypted/undecoded content instead of an unproven length error; promoted with !5950. |
| !5947 | merged | Scanned | TVB LZ decompressors reject zero-sized inputs before calling decompression logic; corroborates helper preconditions. |
| !5946 | merged | Scanned | macOS libssh dependency bump; maintenance only. |
| !5945 | merged | Discussion-focused | SSH feature series received rebase, static-analysis, filter-name, and formatting review; mostly corroborative submission/review evidence. |
| !5944 | merged | Deep | Stig Bjørlykke catches incompatible `void *data` meanings between HTTP/2 and DTAP; Pascal Quantin removes unsafe forwarding. Promoted to subdissector context notes. |
| !5943 | merged | Corroboration | Bug-fix bundle exposes the broken multi-commit CI revision expression later fixed by !5954. |
| !5942 | merged | Scanned | Visual Studio 2022 version-reporting update; no durable new convention. |
| !5941 | merged | Backport | release-3.4 backport of S1AP/NGAP column-fence correction. |
| !5940 | merged | Backport | release-3.6 backport of S1AP/NGAP column-fence correction. |
| !5939 | merged | Scanned | Master S1AP/NGAP/E2AP column-fence correction in templates and regenerated sources; consistent generated-source maintenance. |
| !5938 | merged | Scanned | SCTP indentation cleanup only. |
| !5937 | merged | Deep | Fixes external plugin links after `wmem_alloc()` moved libraries by updating `wireshark.pc`; promoted to ABI/link-interface notes. |
| !5936 | merged | Scanned | TLCP standard update with a supplied representative pcap; standards/sample corroboration. |
| !5935 | closed | Superseded | Same link fix as !5937; closed because the source-branch setup blocked maintainer edits. Uli Heilmeier's workflow feedback is retained at lower weight. |
| !5934 | closed | Superseded | Original TLCP submission from protected master; Jaap Keuter explicitly directs the contributor to use a branch. Merged successor !5936 is authoritative. |
| !5933 | merged | Scanned | Gerald Combs zero-initializes the full zlib stream after Coverity found an untouched member; supports whole-object initialization when partial init is unsafe. |
| !5932 | merged | Scanned | Adds an option to omit secondary data sources from printed/exported hex dumps; feature-specific. |
| !5931 | closed | Duplicate | Duplicate GTP extended-common-flags work; merged !5927 is the accepted implementation. |
| !5930 | merged | Scanned | Optional display of O-RAN I/Q samples; feature-specific. |
| !5929 | merged | Scanned | Adds PTP interval analysis; protocol-analysis feature without reusable review feedback. |
| !5928 | merged | Scanned | TVB search helpers handle a NULL contiguous pointer by returning not-found instead of searching; defensive API handling. |
| !5927 | merged | Scanned | Accepted GTP extended-common-flags implementation; Jaap Keuter catches an incorrect issue reference, reinforcing submission metadata accuracy. |
| !5926 | merged | Deep | Reviewer discussion verifies that zero/NULL initializers used to fix PTP warnings are semantically valid, not merely analyzer appeasement. Promoted. |
| !5925 | merged | Scanned | CMake indentation cleanup only. |
| !5924 | merged | Scanned | John Thacker regularizes RPM CMake macros across RHEL/SUSE versions; packaging-specific. |
| !5923 | merged | Discussion-focused | FPP mCRC/state correction; Uli Heilmeier catches naming and whitespace issues, mostly protocol-specific. |
| !5922 | merged | Deep / high-authority | João Valverde rejects hiding bad caller input behind plausible empty output; Guy Harris argues programmer failures should surface as dissector bugs. Accepted fix repairs callers and asserts the helper precondition. Promoted. |
| !5921 | merged | Scanned | Clarifies an RPM macro comment; documentation only. |
| !5920 | merged | Deep | João Valverde agrees that changing function return semantics merely to silence a dead-store warning is worse than explicitly marking an intentional unused value. Promoted. |
| !5919 | merged | Backport | release-3.4 ISAKMP spelling-only backport. |
| !5918 | merged | Backport | release-3.6 ISAKMP spelling-only backport. |
| !5917 | merged | Scanned | Master ISAKMP spelling-only correction. |
| !5916 | merged | Scanned | Reorders fallback assignments before noreturn assertions to satisfy unreachable-code analysis; compiler-flow cleanup. |
| !5915 | merged | Scanned | 802.11 comment typo only. |
| !5914 | merged | Scanned | RPM build-directory/path normalization across distributions; packaging-specific. |
| !5913 | merged | Discussion-focused | SOCKS-over-TLS support merged as a complete scoped increment while harder nested TLS support was explicitly deferred. |
| !5912 | merged | Deep | Gerald Combs guarantees Kafka's optional string output is initialized before every parse/error path. Promoted to output-parameter notes. |
| !5911 | merged | Deep / high-authority | Guy Harris attaches Wiretap private state and cleanup callbacks before later failures can occur, ensuring ordinary teardown frees partial initialization. Promoted to cleanup/ownership notes. |
