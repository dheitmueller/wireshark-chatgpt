# Wireshark MR review findings — !3011–!3060

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are weighted more heavily than closed/superseded work. Maintainer-authored and maintainer-reviewed guidance is weighted according to authority and specificity.

| MR | Outcome | Depth | Findings |
|---|---|---|---|
| !3060 | merged | Scanned | Pascal Quantin fix for NAS 5GS Non-3GPP NW policies IE dissection. Protocol-specific correctness fix; no new cross-cutting convention. |
| !3059 | merged | Scanned | NVMe dead-store/bounds cleanup by Alexis La Goutte. Uses the clamped byte count rather than the original maximum when adding log-page items. Corroborates existing effective-length guidance. |
| !3058 | merged | Scanned | Final RSerPool statistics additions and documentation screenshots. Feature/documentation work; no durable new convention. |
| !3057 | merged | Scanned | Additional RSerPool statistics additions. No distinct cross-cutting lesson. |
| !3056 | merged | Scanned | PROFINET multiple-API handling correction. Protocol-specific state/iteration fix. |
| !3055 | merged | Discussion-focused | NSIS protobuf installation changed to install the full generated directory instead of enumerating individual files. Graham Bloice immediately checked the equivalent WiX path, reinforcing that packaging changes should be audited across all supported installers. |
| !3054 | merged | Scanned | ErlDP handshake flags dissection. Protocol feature only. |
| !3053 | merged | Deep / high-authority | John Thacker converts `dissect_dvb_s2_bb()` to the standard `dissector_t` signature and passes a TVBuff subset beginning at the child protocol boundary. This removes parent-relative offset plumbing and makes the child reusable from other carriers. Strong child-dissector boundary/API evidence. |
| !3052 | merged | Scanned | Zigbee ZDO deprecated-command expert info; review only caught terminology. No broader convention. |
| !3051 | merged | Discussion-focused | ErlDP handshake tag corrected from `FT_STRING` to `FT_CHAR` with `BASE_HEX` so the field type matches the API used to retrieve it. Corroborates typed-item registration discipline. |
| !3050 | merged | Corroboration | NGAP generated ASN.1/conformance/template sources and generated C changed together. Reinforces generated-source workflow already recorded. |
| !3049 | merged | Scanned | CMake protobuf support corrected for multiple files. Graham Bloice compared the implementation to the analogous radius path. No new general convention. |
| !3048 | merged | Discussion-focused | E2AP ORAN dissector. Anders Broman caught a format warning and questioned multiple SCTP PPI registrations; unresolved/uncertain registrations were disabled rather than guessed. Useful caution against claiming dispatch identifiers without understanding their semantics. |
| !3047 | merged | Corroboration | Follow-up adoption/fixup of shared `ENC_APN_STR` for NAS 5GS. Reinforces centralized wire-encoding helpers and source-wide follow-through. |
| !3046 | merged | Scanned | DCT2000 longer line/PDU support. Accepted successor to closed !3041; implementation precedent resides here. |
| !3045 | merged | Deep | Introduces shared `ENC_APN_STR` support and migrates several APN/DNN dissectors away from duplicated label-to-dot rewriting. Pascal Quantin identified another consumer for follow-up. Durable lesson: centralize recurring wire encodings in core TVBuff/proto APIs and audit all consumers. |
| !3044 | merged | Deep / high-authority discussion | João Valverde adds an always-enabled bounds assertion; Guy Harris challenges both the overly specific name and the semantic ambiguity between assertions that may be compiled out and checks that always abort. Durable API lesson: names should reveal whether a condition is a developer assertion or an unconditional process-fatal invariant check. |
| !3043 | merged | Scanned / high-authority | Guy Harris fixes an AUTHORS address to the repository's anti-spam form. Administrative only. |
| !3042 | merged | Scanned | Bluetooth Mesh opcode support. Protocol feature only. |
| !3041 | closed | Superseded | First DCT2000 longer-line submission. Closed and superseded by merged !3046; down-weighted. |
| !3040 | merged | Discussion-follow-up | Removes MaxMindDB `PACKAGE_VERSION` compile-time use after !3026 discussion identified it as an unsuitable/publicly colliding macro. Stronger lesson is captured with !3026. |
| !3039 | merged | Deep | Large WoW dissector refactor received detailed style review. Anders Broman preferred ordinary grouped static `hf_` variables over structs; Alexis La Goutte preferred arranging helper definitions to avoid unnecessary forward declarations and following editor modeline/editorconfig formatting. Windows CI also exposed an uninitialized-local warning that led to simplifying ownership of a temporary tree variable. |
| !3038 | merged | Deep | OER malformed bit-string handling. Anders Broman caught that the first fix did not prevent overflow. Final code rejects invalid unused-bit counts, advances according to the encoded element length, and writes only while the destination array has capacity. Durable lesson: input-consumption progress and destination-buffer capacity are separate invariants. |
| !3037 | merged | Scanned | QUIC unencrypted padding handling. Protocol-specific. |
| !3036 | merged | High-authority backport | Guy Harris release-3.4 backport of !3034. Duplicates the same structured-model cleanup. |
| !3035 | merged | Scanned | CIP vendor-string whitespace correction. No durable lesson. |
| !3034 | merged | Deep / high-authority | Guy Harris removes stringify-then-parse plumbing from Qt PortsModel population and appends structured rows directly from the structured service map. Durable architecture lesson: preserve typed/structured data through internal layers rather than serializing it to an ad-hoc text intermediate only to parse it again. |
| !3033 | merged | Corroboration | NRPPA ASN.1 update modifies authoritative ASN.1/configuration plus generated output. Existing generated-code guidance covers it. |
| !3032 | merged | Scanned | Service-name lookup tables are torn down and rebuilt on profile switch. Reinforces that profile-scoped configuration-derived caches must be refreshed when profiles change. |
| !3031 | merged | Scanned | Earlier QUIC padding handling submission. Merged but no distinct review lesson beyond protocol correctness. |
| !3030 | merged | Deep | IMAP STARTTLS state fix. `ssl_requested` was being cleared for every frame and is now cleared only when the expected OK response drives the state transition. Attached capture plus master keys supported verification. Durable state-machine lesson: clear/request transition state on the protocol event that consumes it, not merely on the next packet. |
| !3029 | merged | Deep | FTP AUTH TLS support. Conversation state records the AUTH TLS request and TLS handoff occurs only after a 234 response. Contributor supplied a capture and TLS master keys. Strong protocol-state plus encrypted-test-vector example. |
| !3028 | merged | Corroboration | LCS-AP ASN.1 update with regenerated output. Existing generated-code guidance applies. |
| !3027 | closed | Superseded | Earlier IMAP TLS-state submission. CI/account discussion only; merged !3030 is the accepted implementation. |
| !3026 | merged | Deep / high-authority review | Adds LZ4/Zstd/MaxMind version reporting. Guy Harris and João Valverde distinguish compile-time feature/header version from runtime shared-library version: `Compiled with` describes the build environment while `Running with` must query the runtime library when an API exists. Windows and Ubuntu CI also exposed missing include-directory propagation and older dependency API availability. |
| !3025 | merged | Deep | Fuzz-found Sparkplug crash. Graham Bloice correctly separated TVBuff length from the MQTT-provided topic string passed through dissector `data`; the accepted fix validates the ancillary context pointer and uses `g_strcmp0` for nullable split results. Durable lesson: fix the actual failing input contract, not an unrelated packet-length proxy. |
| !3024 | merged | Scanned | HTTP build fix when zlib/brotli are disabled. Build-configuration correctness only. |
| !3023 | merged | Scanned | NGAP/XnAP/NAS-5GS E212 GUAMI field choices. Protocol/filter organization. |
| !3022 | merged | Scanned | NAS EPS adoption of E212_GUMMEI. Follow-up consistency work. |
| !3021 | merged | Discussion-focused | John Thacker adds specific MCC/MNC fields for S1AP/X2AP; Pascal Quantin asks for the same semantic field treatment across NAS/5GS-related dissectors. Reinforces cross-dissector consistency when fields represent the same protocol identity. |
| !3020 | merged | Deep | CredSSP/RDP/Kerberos fixes with captures and keys. David Fort points out that `errorCode` and `clientNonce` are version-dependent; the accepted code gates them on negotiated CredSSP version instead of merely relying on ASN.1 OPTIONAL. Authoritative ASN.1/config/template/generated sources were updated together. |
| !3019 | merged | Scanned | BACnet Secure Connect implementation. Large feature, little reusable review guidance in the snapshot. |
| !3018 | merged | Scanned | Automatic data update. No engineering convention. |
| !3017 | merged | Scanned | Automatic data update. No engineering convention. |
| !3016 | merged | Scanned | Automatic data/translation/authors update. No engineering convention. |
| !3015 | merged | Scanned | HTTP disabled decompression is no longer treated as an error. Configuration/diagnostic correctness only. |
| !3014 | merged | Deep / high-authority | Guy Harris removes non-exported `wsutil/plugins.h` from exported `epan/epan.h`. Public headers should not leak implementation-only dependencies just because current users happen to need them elsewhere. |
| !3013 | closed | Superseded | Draft IEEE 1905 work with spec ambiguity; explicitly reported as merged later in !12314. Down-weighted and not used as implementation precedent. |
| !3012 | merged | Corroboration | XnAP-specific MCC/MNC fields and generated output. Existing generated-code/filter-semantic guidance applies. |
| !3011 | merged | Corroboration | M2AP/M3AP ECGI-specific MCC/MNC fields with generated-source updates. Existing guidance applies. |

## Durable findings promoted from this run

- Reusable child dissectors should receive a TVBuff already bounded/positioned to the child protocol and use the standard `dissector_t` contract where practical (!3053, John Thacker).
- Core APIs should centralize recurring wire encodings instead of forcing several dissectors to hand-roll equivalent mutation/decoding logic (!3045, !3047).
- Distinguish ordinary assertions that may be disabled from unconditional fatal invariant checks, and make the behavior legible in the API name (!3044, especially Guy Harris review).
- Malformed-input parsing must distinguish how many input bytes are consumed from how many decoded elements fit in the destination storage (!3038).
- Preserve structured data across internal layers; avoid serializing to an ad-hoc string only to immediately parse it back into structure (!3034/!3036, Guy Harris).
- Stateful protocol upgrades such as STARTTLS must transition state on the protocol response/event that consumes the pending request; attach decryptable captures and keys for verification when possible (!3029, !3030).
- Compile-time and runtime dependency versions answer different questions. Report the build-time header/feature under compiled-version information and query the loaded runtime library for running-version information when the library API permits it (!3026, Guy Harris + João Valverde).
- Fuzz fixes should validate the actual failing contract. Ancillary dissector `data` can be NULL/empty independently of TVBuff length; do not add an unrelated payload-length check as a proxy (!3025).
- Version-dependent OPTIONAL protocol fields should still be gated by the negotiated/specification version when the standard says they are not legal in earlier versions (!3020).
- Public exported headers must not include implementation-only/non-exported headers solely as incidental transitive dependencies (!3014, Guy Harris).

No SMPTE 291/VANC packet type was encountered in this batch.
