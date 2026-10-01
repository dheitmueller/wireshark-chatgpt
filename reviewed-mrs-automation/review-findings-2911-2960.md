# Wireshark MR review findings — !2911–!2960

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs in this batch are merged, so no closed/superseded implementation needed to be down-weighted. Maintainer-authored/reviewed guidance was weighted by specificity and authority. No direct Guy Harris review comment appears in this batch; the strongest review guidance comes from Gerald Combs, Anders Broman, Pascal Quantin, João Valverde, and Alexis La Goutte.

| MR | Outcome | Findings |
|---|---|---|
| !2960 | merged | **GitLab CI: Fix our fuzzing resource group.** — Merged release-3.4 follow-up gives the stable branch its own fuzz resource group rather than sharing the master serialization key; strong corroboration for branch-scoped CI fuzz scheduling. |
| !2959 | merged | **GitLab CI: Adjust the Documentation rules.** — Merged CI usability change avoids making an otherwise-successful pipeline appear blocked merely because optional documentation can be run manually. |
| !2958 | merged | **Tools: Show only filenames when fuzzing.** — Merged fuzz-driver output cleanup; no distinct project-wide convention beyond keeping long-running fuzz output actionable. |
| !2957 | merged | **GitLab CI: Add fuzzing to the 3.2 branch.** — Merged stable-branch fuzzing enablement. Together with !2956/!2960, shows maintained branches should run equivalent fuzz coverage without sharing one global resource lock. |
| !2956 | merged | **GitLab CI: Add fuzzing to the 3.4 branch.** — Merged stable-branch fuzzing enablement; same scheduling/testing lesson as !2957. |
| !2955 | merged | **GitLab CI: Give our jobs proper names.** — Merged CI naming cleanup; no new durable convention. |
| !2954 | merged | **GitLab CI: Give our jobs proper names.** — Merged stable-branch counterpart; no additional lesson. |
| !2953 | merged | **GitLab CI: Restore the ability to run pipelines from the web UI.** — Merged CI rules correction preserving explicit web-triggered workflows. |
| !2952 | merged | **GitLab CI: Restore the ability to run pipelines from the web UI.** — Merged branch counterpart; no additional lesson. |
| !2951 | merged | **GitLab CI: Give our jobs proper names.** — Merged CI naming cleanup; no new durable convention. |
| !2950 | merged | **GitLab CI: Restore the ability to run pipelines from the web UI.** — Merged CI rules correction; no additional lesson. |
| !2949 | merged | **GitLab CI: Simply our fuzz run times.** — Merged simplification gives ASAN and Valgrind fuzz stages explicit run budgets instead of deriving them from overall job age. Useful operational history, but not a new architecture rule. |
| !2948 | merged | **IEEE 802.11: indicate termination for FTM Response** — Merged protocol-field enhancement; no cross-cutting review lesson. |
| !2947 | merged | **http: Add dissection of HTTP2-Settings** — Deep. Anders Broman identified malformed/base64url uncertainty and suggested TRY/CATCH around the nested HTTP/2 decoder; Pascal Quantin requested a subtree. The merged code decodes into a child tvbuff/data source, calls a state-neutral HTTP/2 settings entry point, and contains child exceptions at the HTTP parent boundary. |
| !2946 | merged | **ITS: use custom formatters for better readability** — Merged generated ITS presentation update changes ASN.1 conformance/template inputs and generated C together, corroborating the source-of-truth/regeneration rule. |
| !2945 | merged | **PFCP: add dissector for Broadband Forum TR-459** — Deep. Anders Broman required stale Diameter naming to be fixed and vendor fields to use a protocol+enterprise namespace such as pfcp.bbf.*; the contributor applied it. Strong naming guidance for enterprise-specific fields. |
| !2944 | merged | **CMake: Enable AUTO{MOC,UIC,RCC} for the qtui target explicitly.** — Deep historical precursor. Gerald Combs tested CMake 3.20.0 failure after João Valverde warned of the bug and retained a narrow global workaround only for 3.20.0/3.20.1 while using target properties otherwise. Later !3004/!3008 remain the stronger/current precedent. |
| !2943 | merged | **GitLab CI: Install valgrind.** — Merged fuzz/CI dependency support; part of the broader fuzzing series. |
| !2942 | merged | **GitLab CI: Fix the fuzzing before and after scripts.** — Merged fuzz-job orchestration correction; corroborates treating harness setup/teardown as part of fuzz correctness. |
| !2941 | merged | **GTPv2: add dissection of Mapped UE Usage Type IE** — Merged protocol-specific addition; no new cross-cutting rule. |
| !2940 | merged | **HTTP2: Make it possible to configure a port range.** — Merged configurability enhancement; no new project-wide convention. |
| !2939 | merged | **ASAP and ENRP statistics improvements** — Merged UI/docs work. Alexis La Goutte requested much smaller screenshots; the contributor recompressed them substantially. Useful documentation-asset hygiene, but too narrow for a new notebook rule. |
| !2938 | merged | **GitLab CI: Add Valgrind and randpkt fuzzing.** — Deep. Gerald Combs builds scheduled ASAN, randpkt, and Valgrind stages, preserves the failing capture plus fuzz stderr, uploads failure artifacts, and serializes the expensive fuzz resource. Strong CI fuzz reproducibility evidence. |
| !2937 | merged | **GitLab CI: Fix a path.** — Merged CI repair; no durable lesson. |
| !2936 | merged | **GSM A-bis/OML: show Manufacturer ID in vendor-specific messages** — Merged protocol-specific enhancement; no new cross-cutting rule. |
| !2935 | merged | **packet-wow: Correct protocol_version field** — Merged narrow field correction; no new project-wide convention. |
| !2934 | merged | **GitLab CI: Fill in fuzz-test.** — Merged fuzz CI expansion; part of the accepted fuzzing infrastructure series. |
| !2933 | merged | **Fix RNR TBTT** — Merged IEEE 802.11 correction with simple approval; no distinct durable lesson. |
| !2932 | merged | **WSDG: Update Qt and MSVC versions** — Merged documentation maintenance; no new convention. |
| !2931 | merged | **IEEE 802.11: fix spelling for TBTT** — Merged spelling correction; no new convention. |
| !2930 | merged | **Qt UI: fix AutoUic warning 'The name label (QLabel) is already in use'** — Merged UI build-warning cleanup; no cross-cutting rule. |
| !2929 | merged | **Revert "GitLab CI: Try switching Windows builds back to Qt 5.15.1."** — Merged historical build correction. Discussion about the then-current Windows CMake/Qt source of truth is time-specific and is not promoted as a current rule. |
| !2928 | merged | **R09: new dissector for R09.x public transport priority telegrams** — Deep. Alexis La Goutte pointed out an existing tvb_get_bcd_string helper, which replaced local reinvention, and requested both a sample pcap and release-note entry; the contributor supplied both. |
| !2927 | merged | **packet-selfm.c - Resolve Uninitialized Variable** — Merged follow-up to the Coverity defect found after !2915. Strongly corroborates the rule that a post-merge correctness fix is a new MR rather than another commit on the already-merged change. |
| !2926 | merged | **ITS: add Collective Perception Service (CPS) - ETSI TR 103 562 V2.1.1 (2019-12)** — Deep submission/generated-code evidence. Pascal Quantin states GitLab does not require the old Gerrit Change-Id trailer and tells the contributor to update hooks from tools/. The accepted change updates ASN.1 inputs/template and generated dissector together. |
| !2925 | merged | **GRPC: Register both tables streaming_content_type/media_type** — Merged dispatch registration update; no new durable convention. |
| !2924 | merged | **Qt: Protocol Hierarchy - protocol abbrev tooltip** — Merged UI behavior discussion exposed platform/Qt-version differences. The 2021 buildbot remarks are historical and not promoted as current build policy. |
| !2923 | merged | **GitLab CI: Add a minimal fuzzing job.** — Merged beginning of the CI fuzzing series later expanded by !2934/!2938. |
| !2922 | merged | **GitLab CI: Miscellaneous updates.** — Merged CI maintenance; no distinct lesson. |
| !2921 | merged | **GitLab CI: Miscellaneous updates.** — Merged CI maintenance; no distinct lesson. |
| !2920 | merged | **GitLab CI: Fix a typo.** — Merged trivial CI fix. |
| !2919 | merged | **GitLab CI: Fix an upload command.** — Merged CI artifact-upload fix; no new convention. |
| !2918 | merged | **GitLab CI: Distribute our documentation.** — Merged CI documentation artifact support; no new convention. |
| !2917 | merged | **packet-iec104.c - Add IEC 60870-5-103 Protocol Dissection** — Merged feature with three focused sample pcaps covering supported ASDU types and Decode As instructions. Strong corroboration of capture-based validation for dissector additions. |
| !2916 | merged | **GitLab CI: Move common RPM declarations to .build-rpm.** — Merged CI deduplication; no new durable convention. |
| !2915 | merged | **packet-selfm.c - Use proto_tree_add_time where appropriate** — Deep submission-process evidence. Pascal Quantin suggested removing obsolete temporaries; after merge Coverity found a legitimate uninitialized-variable defect. When the author asked whether to append another commit, Pascal explicitly required a new MR; merged !2927 is that follow-up. |
| !2914 | merged | **GitLab CI: Publish the API reference.** — Merged CI documentation publishing; no new convention. |
| !2913 | merged | **GitLab CI: Fix our Coverity submission URLs.** — Merged static-analysis CI maintenance; no new convention. |
| !2912 | merged | **GitLab CI: Try to fix Coverity submissions.** — Merged static-analysis CI maintenance; no new convention. |
| !2911 | merged | **RTP Player: Player is able to skip silence during playback** — Merged Qt/RTP Player feature with documentation. No substantive maintainer review in the corpus snapshot and no new project-wide convention extracted. |

## Durable findings promoted from this run

- !2947: when a parent decodes an encoded/embedded child structure, establish a child tvbuff/data source at the semantic boundary and contain child exceptions when malformed child data should not destroy the parent dissection. If the child normally mutates session state, expose a state-neutral entry point for callers that only have the embedded structure.
- !2945: vendor/enterprise-specific display-filter fields should live under a stable protocol + enterprise namespace such as `pfcp.bbf.*`, rather than polluting a flat protocol namespace or retaining stale names copied from another dissector.
- !2938 together with !2923/!2934/!2942/!2943/!2956/!2957/!2960: recurring fuzz CI should preserve the failing capture and harness diagnostics, cover materially different engines such as sanitizers/randpkt/Valgrind, and serialize only jobs that contend for the same resource. Maintained branches need distinct resource-group identities.
- !2926: GitLab-era Wireshark commits should not carry obsolete Gerrit `Change-Id` trailers. Use the project-provided hooks under `tools/` so local commit metadata matches the current submission workflow.
- !2915 + !2927: once an MR has merged, a subsequently discovered correctness defect is fixed in a new MR. Do not append another commit to the already-merged review unit.
- !2928 and !2917: protocol additions should reuse existing tvbuff/proto helpers where they express the same wire operation, and focused sample captures remain important review evidence. !2928 also reinforces the release-note expectation for a newly supported protocol.
- !2946 and !2926: generated ASN.1 changes continue to update authoritative conformance/template inputs and regenerated C together; this corroborates the existing source-of-truth rule.
- !2944 is retained as historical build evidence only; the later reviewed !3004/!3008 series is the stronger/current Qt AUTOGEN precedent.

No SMPTE ST 291/VANC packet type was encountered.
