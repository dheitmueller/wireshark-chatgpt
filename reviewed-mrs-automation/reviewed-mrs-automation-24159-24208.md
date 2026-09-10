# Wireshark MR review batch !24159–!24208

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run files in `reviewed-mrs-automation/`, including the preserved !17571–!17620 batch. These are the 50 highest-numbered corpus MRs not present in that reviewed set. The list below is authoritative for exactly what this run reviewed; no unlisted number is implied by the filename range.

| MR | Review status | Notes |
|---|---|---|
| !24208 | Deep | Merged QCDIAG partial revert. Michael Mann explicitly rejected non-idiomatic dissector rewrites made only to placate Coverity false positives and favored improving analyzer modeling; also reaffirmed packet-scope `wmem_strbuf_t`/`pinfo->pool`. Promoted static-analysis review guidance. |
| !24207 | Scanned | DECT NR Coverity cleanup removes a redundant NULL test where the call contract already requires context. Useful counterexample: fix real/static contract issues, but no separate rule beyond !24208. |
| !24206 | Discussion-focused (closed/superseded) | Earlier full QCDIAG revert attempt. Down-weighted because it was closed; the later merged !24208 carries the accepted outcome and rationale. |
| !24205 | Deep | TLS ClientHello release-4.6 backport uses a subset TVB so remaining-length APIs own the bounds/overflow reasoning and removes bogus offset/length plumbing. Strong corroboration of existing subset-TVBuff guidance. |
| !24204 | Deep | NetXray reader caps expected trailing padding to actual captured length before subtraction, preventing unsigned underflow. Corroborates existing subtract-before-compare/remaining-capacity guidance. |
| !24203 | Deep | dumpcap release-4.4 SHB hardening grows the receive buffer to the block's required size, validates the SHB minimum, and defers endian-sensitive checks until byte order is known. Corroborates input-size/capacity validation guidance. |
| !24202 | Scanned | Sibling/backport of dumpcap SHB buffer-size hardening; no additional convention beyond !24203 and the corresponding master fix. |
| !24201 | Scanned | `fuzz-test.sh` command-line cleanup and help/parameter normalization. Tooling maintenance; no durable review convention added. |
| !24200 | Scanned | `format_size` unit coverage cleanup. Straightforward utility completeness fix; no new durable lesson. |
| !24199 | Scanned | DECT NR sequence-number filter renamed to the project's common `.sn` form. Narrow naming consistency fix. |
| !24198 | Scanned | Automatic data/translation update; no reusable engineering lesson. |
| !24197 | Scanned | Automatic data/translation update; no reusable engineering lesson. |
| !24196 | Scanned | Automatic data/translation update; no reusable engineering lesson. |
| !24195 | Scanned | DECT NR DLC extension-header/IE decoding expansion. Accepted protocol work without a distinct reusable review lesson in this snapshot. |
| !24194 | Scanned | MKA splits live- and potential-peer-list dissection once their behavior diverges. Sensible cohesion refactor, but no new general rule beyond existing source/helper separation guidance. |
| !24193 | Scanned | DECT NR MAC IE length-handling fix; bounds/length semantics corroborate existing parser guidance. |
| !24192 | Scanned | Sibling DECT NR MAC IE length-handling change; no additional lesson beyond !24193. |
| !24191 | Deep | John Thacker adds checked increment/decrement-by-N dissection-depth APIs, makes depth unsigned for checked-underflow semantics, and fixes ASN.1 generated depth accounting. Strong corroboration of existing exact/scoped recursion-depth guidance. |
| !24190 | Scanned | AUTOSAR DLT removes a vestigial loop that cannot iterate. Straightforward control-flow cleanup. |
| !24189 | Scanned | QUIC multipath draft-19 update. Protocol-version maintenance; no new reusable review convention extracted. |
| !24188 | Deep | John Thacker fixes TLS ECH loop progress when an extension has zero size and rejects a forbidden nested extension before copying could enlarge ClientHelloInner beyond ClientHelloOuter. Strong corroboration of monotonic-progress and pre-copy structural validation rules. |
| !24187 | Scanned | User-guide filter-string formatting cleanup. Documentation-only. |
| !24186 | Scanned | Zigbee RF4CE unencrypted-path subset-TVBuff change; sibling/backport, reinforcing existing subset-TVBuff guidance. |
| !24185 | Deep | Zigbee RF4CE replaces an unnecessary unencrypted-path copy with a bounded subset TVB, making the child TVB carry size semantics directly. Corroborates existing canonical subset-TVBuff rule. |
| !24184 | Scanned | Reassembly documentation typo/name cleanup. Documentation-only. |
| !24183 | Deep | John Thacker's merged asn2wrs change conservatively treats unconstrained open types as potentially cyclic so generated dissectors apply dissection-depth checks. Corroborates the notebook's existing ASN.1 recursion-protection guidance. |
| !24182 | Scanned | SMB1 NEGOTIATE response description improvements. Presentation/diagnostic clarification; no general convention added. |
| !24181 | Scanned | Earlier/sibling Zigbee RF4CE subset-TVBuff change; accepted direction is represented by !24185/!24186 and existing notebook guidance. |
| !24180 | Scanned | Generated HI2Operations proto/hf variables made static. Straightforward internal-linkage cleanup. |
| !24179 | Deep | Martin Mathieson replaces separate fetch+tree-add patterns across dissectors with return-value tree helpers; Anders Broman merged it. Strong corroboration of the notebook's existing fetch/convert-once rule. |
| !24178 | Scanned | HTTP adds expert warning/validation for malformed response status-code tokens. Reinforces explicit malformed-input diagnostics without a new architecture rule. |
| !24177 | Scanned | MKA centralizes parameter-set size and padding handling. Cohesion/deduplication refactor; no distinct new convention. |
| !24176 | Scanned | Qt SystemUpdate handler/check expansion. Platform UI/update logic; no reusable Wireshark-core convention extracted. |
| !24175 | Scanned | ICMP adds expert warnings for malformed extension-object lengths. Reinforces normal malformed-data reporting. |
| !24174 | Deep | Michael Mann removes packet-triggerable `DISSECTOR_ASSERT`s and replaces them with normal validation/failure paths. Promoted durable rule: assertions represent developer invariants, not malformed-capture conditions. |
| !24173 | Scanned | iLBC multiframe decoded-length fix; sibling/backport. No additional convention. |
| !24172 | Scanned | iLBC multiframe decoded-length fix; sibling/backport. No additional convention. |
| !24171 | Scanned | DCP ETSI validates equal fragment sizes when Reed-Solomon is in use. Protocol-specific reassembly validation; consistent with existing reassembly invariants. |
| !24170 | Scanned | Sibling/backport of DCP ETSI equal-fragment-size validation; no additional lesson. |
| !24169 | Scanned | Qt Follow Stream preserves configured font while zooming. UI behavior fix; no durable core convention. |
| !24168 | Scanned | Sibling/backport of Follow Stream font-preservation fix. No additional lesson. |
| !24167 | Scanned | iLBC multiframe decoded-length fix; sibling/backport. No additional convention. |
| !24166 | Scanned | Show Packet Bytes font-zoom behavior. UI-specific fix; no durable core convention. |
| !24165 | Scanned | TLS registers the well-attested but unregistered Ivanti VPN HTTP Upgrade token for TLS-in-TLS handling. Protocol registration case-specific; no broader rule promoted. |
| !24164 | Scanned | Sibling/backport of DCP ETSI equal-fragment-size validation; no additional lesson. |
| !24163 | Scanned | ASN.1 follow-up fixes duplicate `lenE` advancement. Parser bookkeeping bug; reinforces exact offset accounting without a new rule. |
| !24162 | Scanned | Qt avoids restoring stale column state on capture-file open. UI state-lifecycle fix; no new general convention. |
| !24161 | Scanned | RTP Streams constructs tree items using the current Time-of-Day setting. UI state synchronization; no durable core rule. |
| !24160 | Scanned | Sibling/backport of RTP Streams Time-of-Day initialization fix. No additional lesson. |
| !24159 | Scanned | extcap chooses the Windows SSH configuration location according to libssh version semantics. Compatibility-specific integration fix; no general Wireshark convention added. |

## Durable findings promoted

1. **Assertions are for programmer invariants, not hostile/malformed capture conditions** (!24174).
2. **Investigate static-analysis warnings, but do not distort established Wireshark idioms solely to silence false positives; improve analyzer modeling/annotations when appropriate** (!24208, with !24207 illustrating the complementary legitimate-warning case).

The remaining strong MRs primarily corroborated notebook rules already recorded for subset TVBs, checked length arithmetic, monotonic parser progress, recursion/depth accounting, reassembly invariants, and fetch-once tree APIs.
