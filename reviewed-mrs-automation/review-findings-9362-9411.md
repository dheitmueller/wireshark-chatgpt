# Wireshark MR review findings: !9362-!9411

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed MRs were examined, newest to oldest. Merged changes are treated as stronger evidence than closed or superseded work. Maintainer-authored changes and direct maintainer review, especially from Guy Harris, John Thacker, Gerald Combs, Martin Mathieson, Anders Broman, Gilbert Ramirez, and other core maintainers, receive greater weight.

Status mix: 48 merged; 2 closed/unmerged (!9371 and !9362). Merged !9393 and !9372 are retained mainly as negative evidence because later accepted work or post-merge reports showed that their original behavior was wrong.

## Strong durable findings

### Dialog completion follows persistence, not vice versa

Merged master !9394, authored by John Thacker, removes an independent Coloring Rules dialog acceptance path and makes successful completion depend on writing the rules first. The two stable backports !9395 and !9396 were merged by Guy Harris. The practical reason is that closing the dialog can trigger packet-list recoloring, so downstream work must not observe a successful transition before persistent state exists.

Promoted to `qt-persistence-transaction-conventions.md`.

### Platform metadata can require cosmetic normalization

Merged !9392, authored and merged by João Valverde, trims CPU model strings before displaying version information. Guy Harris's discussion is particularly useful: Intel processor brand strings may contain leading blanks, and Windows can preserve formatting that another operating system removes. Human-readable host metadata therefore cannot assume identical cosmetic normalization across platforms.

Promoted to `platform-metadata-string-conventions.md`.

### External registries remain authoritative even when labels look stale

Merged !9384 refreshes the CIP vendor list from ODVA. Roland Knall noted that one registered company name was historically stale; the contributor and Alexis La Goutte preferred the ODVA value because it matches the protocol registry and EDS/tooling ecosystem. Gilbert Ramirez similarly treated ODVA's Reserved entries and unusual spelling as authoritative unless the registry changes. Gerald Combs confirmed that non-ASCII UTF-8 in source strings/comments is acceptable when used sparingly.

Promoted to `external-registry-label-conventions.md`.

### CI commit-range checks use an explicit CI revision identity

Merged !9377, authored and merged by Gerald Combs, changes the pre-commit range from ambient HEAD to the CI-provided commit SHA. This complements the notebook's later source-versus-synthetic-merge distinction: first choose which commit identity the check semantically needs, then derive the range from that explicit identity rather than relying on checkout state.

Promoted to `ci-commit-range-conventions.md`.

### Packet-buffer copies should stay inside the packet-buffer API

Merged !9376, authored and merged by Gerald Combs, replaces direct packet-buffer memory views used only as an intermediate copy source with the packet-buffer library's copy operation. Gerald explicitly says the previous uses were technically valid but that direct packet-buffer views should be minimized. Martin Mathieson and Ales Povalac validated the affected protocols, and Ales found another occurrence during review. Merged !9374 independently makes the same style change in ALP.

Promoted to `packet-buffer-copy-conventions.md`.

### Conversation identity should not be synthesized by cloning packet context

Merged !9373, authored by Guy Harris, replaces an EAP path that copied the full packet context and edited addresses/ports with the conversation API designed to supply alternate endpoint values. This is high-authority evidence that packet dissection context and conversation-key construction are separate abstractions.

Promoted to `conversation-endpoint-override-conventions.md`.

### Static-analysis nullable paths need contract review before merge

Merged !9372 added HTTP/2 window tracking. Alexis La Goutte reported a static-analysis null path during review. After merge, John Thacker reported a real regression; the contributor confirmed that an existing helper legitimately invokes the path without an HTTP/2 session and opened !9534. The original merged code is therefore negative evidence: when a warning maps to a legitimate nullable entry path, it should be resolved as a correctness issue rather than deferred as analyzer noise.

Promoted to `static-analysis-null-context-conventions.md`.

### Refactoring built-in dispatch must preserve extension contracts

Merged !9368 initially removed PFCP's enterprise dissector table. Joakim Karlsson pointed out that this removed the ability to register custom enterprise dissectors. The contributor restored it, and Anders Broman stated the architectural principle directly: built-in vendor handling should work through the same registered-dispatch model as external dissectors.

Promoted to `dissector-extension-point-preservation-conventions.md`.

### Process exit codes are semantic API results

Merged !9367, authored and merged by Martin Mathieson, replaces a generic status shared by unrelated failures with distinct statuses for interface, file, filter, capability, initialization, and open failures, and updates dftest/tests to use the same shared vocabulary.

Promoted to `process-exit-code-semantics-conventions.md`.

### Host crypto policy is not disabled to keep a deprecated optional algorithm

Merged !9364 fixes capinfos under FIPS policy. Guy Harris raised the possibility of disabling FIPS, but the contributor noted that some systems require the policy. Gerald Combs instead favored removing RIPEMD160 because it was nonessential and already deprecated elsewhere. The accepted change removes it and updates capinfos documentation.

Promoted to `crypto-policy-compatibility-conventions.md`.

## Additional durable or corroborating observations

- !9411 is a small BGP-LS constant/length fix. The review sequence explicitly delayed approval until CI was launched, corroborating the expectation that even small corrections pass the pipeline before merge.
- !9409 allows UDS and OBD-II to coexist on one transport by handing the lower service-ID space to the OBD-II dissector behind a preference. It is useful protocol-multiplexing evidence, but no substantive maintainer discussion justified a new notebook rule.
- !9408 fixes OBD-II service 0x04's valid zero-parameter request/response shape.
- !9404 corrects Opus TOC values from RFC 6716; !9405 and !9406 are release backports. These are specification-value corrections rather than new architecture guidance.
- !9402 corrects the PFCP QFI mask from seven bits to six according to 3GPP.
- !9399 removes the arbitrary fatal interpretation of repeated SCAP timeout results introduced by !9393. This pair is useful negative evidence that an API's timeout result must be interpreted according to the API contract rather than converted into an invented application-level fatal timer.
- !9398 stops automatically discovering Falco plugins from a nonstandard location and instead requires explicit plugin paths. This moderately supports the build-system principle that ambiguous optional components without a stable installation convention should be explicit configuration rather than guessed discovery.
- !9397 distinguishes the MSYS2/MinGW environment from the ordinary Windows third-party-library build and system Qt plugin behavior. Review caught a wrong condition and a CMake-version dependency.
- !9386 contains Martin Mathieson's direct review of mixed indentation/trailing whitespace and is corroboration for source-hygiene checks.
- !9385 extends the ASN.1 extraction tool for M2AP/M3AP, supporting the existing source-of-truth principle for generated ASN.1 dissectors.
- !9390, !9389, !9388, !9387, !9383, !9382, !9381, and !9380 are accepted specification-driven ASN.1 upgrades and mainly corroborate authoritative-input/regeneration practice.
- !9375 fixes a width mismatch by using an 8-bit local for an 8-bit packet-buffer getter, reinforcing type-width fidelity.
- !9374 also fixes an ALP count loop so that, after one MPEG-TS packet is already reconstructed, the loop processes only the remaining count rather than one extra iteration.
- !9369 handles the MySQL compressed-protocol case where compression is active but a packet's uncompressed-length field indicates that the individual payload is actually uncompressed.
- !9365 is the accepted counterpart to closed draft !9371. It changes Wireshark's debug/assert configuration toward a positive project macro that works more cleanly with CMake generator expressions while respecting the supported CMake baseline. The closed NDEBUG-only design is not treated as accepted architecture.
- !9363 removes a large set of identical PFCP grouped-IE wrapper functions in favor of one shared implementation; review was positive but adds no new rule beyond ordinary deduplication.

## Per-MR ledger notes

| MR | Outcome | Depth | Review note |
|---|---|---|---|
| !9411 | merged | Discussion-focused | BGP-LS IPv6 Neighbor Address length constant corrected; Alexis delayed approval until CI was launched. |
| !9410 | merged | Scanned | MSYS2 README updated to the package-build workflow. |
| !9409 | merged | Deep | UDS/OBD-II overlap handled through subdissector handoff and preference. |
| !9408 | merged | Scanned | OBD-II service 0x04 zero-parameter request/response handling corrected. |
| !9407 | merged | Scanned | Adds MSYS2 PKGBUILD. |
| !9406 | merged | Scanned | Stable backport of Opus TOC timing correction. |
| !9405 | merged | Scanned | Stable backport of Opus TOC timing correction. |
| !9404 | merged | Scanned | Master Opus TOC values corrected from RFC 6716. |
| !9403 | merged | Scanned | MySQL presentation typo fixes. |
| !9402 | merged | Scanned | PFCP QFI mask corrected from 7 to 6 bits. |
| !9401 | merged | Scanned | Stable macOS donation-page packaging backport. |
| !9400 | merged | Scanned | Falco plugin configuration variable renamed and command made explicit. |
| !9399 | merged | Deep | Corrects !9393: SCAP timeout is nonfatal; arbitrary elapsed-time shutdown removed. |
| !9398 | merged | Discussion-focused | Falco plugin paths made explicit because no standard install location exists. |
| !9397 | merged | Discussion-focused | MSYS2 install/relocation logic; review caught condition and CMake baseline details. |
| !9396 | merged | Deep | Stable Coloring Rules persistence-order backport, merged by Guy Harris. |
| !9395 | merged | Deep | Stable Coloring Rules persistence-order backport, merged by Guy Harris. |
| !9394 | merged | Deep | John Thacker master fix makes dialog completion follow successful rule persistence. |
| !9393 | merged | Negative evidence | Introduced arbitrary fatal timeout behavior later corrected by !9399. |
| !9392 | merged | Deep | Cross-platform CPU brand-string padding normalization with Guy Harris analysis. |
| !9391 | merged | Scanned | Diameter copied label corrected. |
| !9390 | merged | Scanned | F1AP authoritative ASN.1 update to v17.3.0. |
| !9389 | merged | Scanned | E1AP authoritative ASN.1 update to v17.3.0. |
| !9388 | merged | Scanned | NRPPa authoritative ASN.1 update to v17.3.0. |
| !9387 | merged | Scanned | XnAP authoritative ASN.1 update to v17.3.0. |
| !9386 | merged | Discussion-focused | Keysight NetFlow update; Martin Mathieson caught whitespace issues. |
| !9385 | merged | Scanned | ASN.1 extraction tooling extended for M2AP/M3AP. |
| !9384 | merged | Deep | ODVA registry fidelity, reserved values, historical names, and UTF-8 source strings discussed by maintainers. |
| !9383 | merged | Scanned | NGAP authoritative ASN.1 update to v17.3.0. |
| !9382 | merged | Scanned | M2AP authoritative ASN.1 update to v17.0.1. |
| !9381 | merged | Scanned | X2AP authoritative ASN.1 update to v17.3.0. |
| !9380 | merged | Scanned | S1AP authoritative ASN.1 update to v17.3.0. |
| !9379 | merged | Scanned | Master macOS donation-page packaging change. |
| !9378 | merged | Scanned | dftest bytecode dump gains optional field-type display flags. |
| !9377 | merged | Deep | Pre-commit CI range anchored to CI commit identity instead of HEAD. |
| !9376 | merged | Deep | Gerald Combs directs packet-buffer copies away from unnecessary direct memory views. |
| !9375 | merged | Scanned | Local integer width matched to the 8-bit packet read. |
| !9374 | merged | Discussion-focused | ALP copy-helper conversion and remaining-count loop correction. |
| !9373 | merged | Deep | Guy Harris replaces copied/mutated packet context with conversation endpoint override API. |
| !9372 | merged | Negative evidence | Static analyzer warned of nullable session path; post-merge regression confirmed by John Thacker. |
| !9371 | closed | Down-weighted | Draft NDEBUG/debug-macro redesign; not accepted and superseded by the accepted !9365 direction. |
| !9370 | merged | Scanned | NAS 5GS specification update to v17.9.0. |
| !9369 | merged | Discussion-focused | MySQL compressed transport now handles individually uncompressed payloads correctly. |
| !9368 | merged | Deep | PFCP enterprise-IE refactor restored dissector-table extension capability after review. |
| !9367 | merged | Deep | Distinct semantic process exit codes and matching tests. |
| !9366 | merged | Scanned | GTPv2 ENSCT bitmask corrected. |
| !9365 | merged | Discussion-focused | Accepted positive debug-macro/CMake direction; closed !9371 is not authoritative. |
| !9364 | merged | Deep | FIPS compatibility resolved by removing RIPEMD160 and updating docs, not weakening platform policy. |
| !9363 | merged | Scanned | PFCP grouped-IE wrappers deduplicated into shared decode logic. |
| !9362 | closed | Down-weighted | HTTP/3/QPACK work explicitly superseded by !9330; Alexis La Goutte agreed to continue there. |

## Frontier

!9361 exists in the corpus, is merged, and was inspected only as the next-frontier probe. It is not part of this run's reviewed set.
