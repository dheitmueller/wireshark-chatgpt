# Review findings: !1010-!1059

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are treated as stronger implementation evidence than closed or superseded submissions. Maintainer-authored and maintainer-reviewed changes, especially Guy Harris guidance, are weighted accordingly.

| MR | Outcome | Depth | Durable review note |
|---|---|---|---|
| !1059 | merged | Deep | John Thacker replaces manual pointer access and nibble shifting with `tvb_new_octet_aligned()`; prefer a TVBuff transformation API when it directly represents the desired wire view. |
| !1058 | merged | Scanned | Corrects the new Base64 TVBuff helper's parameter documentation; documentation-only follow-up. |
| !1057 | merged | Scanned | Guy Harris adds a more usable 3270 specification reference; documentation-only. |
| !1056 | merged | Scanned | OML attribute parsing gains an explicit enclosing length and additional attribute decoding; useful bounded-parser corroboration. |
| !1055 | merged | Deep | Gerald Combs converts the last YACC/Bison grammar to Lemon and removes master build/package/docs dependencies; Guy Harris notes shared support images still need Bison for maintained stable branches. |
| !1054 | merged | Deep | Reuses a libgcrypt cipher context with reset inside a loop instead of repeated open/close, addressing Coverity double-free reporting and needless lifecycle churn. |
| !1053 | merged | Scanned | Bluetooth retransmission context becomes more explicit and avoids calling normal ACK/NESN behavior “wrong”; protocol-analysis/UI wording change. |
| !1052 | merged | Deep | Replaces digest cleanup/re-init inside a loop with reset; same reusable-context lifecycle rule as !1047/!1049/!1054. |
| !1051 | merged | Scanned | Corrects a duplicated NAS EPS protocol value. |
| !1050 | merged | Scanned | MySQL length-encoded fields gain subtrees and stronger Info-column composition; no new cross-cutting rule beyond existing column guidance. |
| !1049 | merged | Deep | Reuses SHA/MD5 contexts with reset instead of destroy/recreate loops; corroborates the crypto-context lifecycle rule. |
| !1048 | merged | Deep | Anders Broman, Alexis La Goutte, and Jaap Keuter steer MQ numeric presentation toward `BASE_HEX_DEC` / `BASE_DEC_HEX` instead of custom table-like formatting; consistency across dissectors is preferred. |
| !1047 | merged | Deep | HMAC context is initialized once, reset/rekeyed for reuse, and cleaned up once; documents the same accepted libgcrypt lifecycle pattern. |
| !1046 | merged | Scanned | TLS key-log comments are ignored rather than reported as malformed/unrecognized input. |
| !1045 | closed | Discussion-focused | Guy Harris explains that a TOCTOU fix must account for Unix-domain sockets, on which `open()` need not work, and questions why this path should remain privileged at all. Strong review guidance; unmerged implementation is not precedent. |
| !1044 | merged | Scanned | Closes a gzip handle on allocation failure; straightforward resource-cleanup fix. |
| !1043 | merged | Scanned | Pascal Quantin fences the Info column before invoking EAP so the nested dissector cannot erase the parent prefix; corroborates existing column-composition guidance. |
| !1042 | merged | Deep | John Thacker replaces a local TBCD digit decoder with `tvb_get_string_enc(... ENC_KEYPAD_ABC_TBCD ...)`; use shared encoding helpers rather than local wire-decoding loops. |
| !1041 | merged | Scanned | Corrects JSON callback/type naming typo. |
| !1040 | merged | Scanned | Adds filterable/subdissectable JSON keys/data and Base64-decoded GTPv2 content; mostly feature-specific. |
| !1039 | merged | Deep | John Thacker/Jörg Mayer add PKCS #11 to macOS bootstrap, including dependency lifecycle, explicit feature tests, and `tshark -v` capability verification; stale prebuilt GnuTLS must be rebuilt to gain the feature. |
| !1038 | merged | Deep / high-authority | Authored and merged by Guy Harris: stops assuming macOS versions are `10.N`; parses major/minor and compares versions semantically for SDK selection. |
| !1037 | merged | Deep | Anders Broman and Alexis La Goutte require XDR alignment bytes to be visible as a Padding field instead of silently skipped; Graham Bloice also enforces Wireshark commit-message conventions. |
| !1036 | merged | Scanned | Exports a GTPv2 IE helper for reuse; no substantive review discussion. |
| !1035 | merged | Scanned | Adds a shared Base64-to-child-TVBuff helper with parent/child ownership and free callback; later documentation typo fixed by !1058. |
| !1034 | merged | Scanned | Removes an unused result assignment found by Clang. |
| !1033 | merged | Deep / high-authority | Guy Harris moves format-specific pipe-open calls after capture format determination, consolidating duplicated control flow. |
| !1032 | closed | Scanned | Empty/accidental dumpcap MR; closed by Guy Harris. No implementation precedent. |
| !1031 | merged | Scanned / high-authority | Guy Harris clarifies dumpcap capture-source and pcapng passthrough comments; documentation-only. |
| !1030 | merged | Deep / high-authority | Guy Harris confines `WSAGetLastError()` to socket I/O and uses the ordinary errno path for pipes; error APIs must match the handle/API domain. |
| !1029 | merged | Deep / high-authority | Guy Harris validates pcapng block-total-length 4-byte alignment immediately after decoding it and reports the bad value clearly before further processing. |
| !1028 | merged | Scanned | DCT2000 LTE/NR context and NRUP handling changes; protocol-specific. |
| !1027 | merged | Deep / high-authority | Guy Harris changes a helper to propagate the underlying numeric error code to its caller instead of losing it behind a Boolean result. |
| !1026 | merged | Scanned | Issue templates explicitly ask for capture files rather than dissector screenshots and for complete version/log information; useful reporting guidance. |
| !1025 | merged | Scanned / high-authority | Guy Harris documents that the pcapng SHB read is synchronous; comment-only. |
| !1024 | merged | Scanned | Stable-branch TShark quiet/tempfile state fix; no new general rule. |
| !1023 | merged | Scanned | GTPv2 F-container now delegates to S1AP container dissectors; protocol-specific. |
| !1022 | merged | Deep | Raises GLib minimum from 2.32 to 2.36 only after the obsolete RHEL 6 floor ceased to matter, then removes dead version branches; corroborates supported-platform-driven dependency floors. |
| !1021 | merged | Scanned | release-3.2 backport of the S1AP transparent-container context fix. |
| !1020 | merged | Scanned | release-3.4 backport of the S1AP transparent-container context fix. |
| !1019 | merged | Deep | The accepted master S1AP fix records the semantic source→target vs target→source container discriminator in ASN.1/conformance inputs and regenerates output, rather than relying on broader outer message type. |
| !1018 | merged | Scanned | Stable-branch QUIC feature-guard compilation fix for builds without libgcrypt AEAD. |
| !1017 | closed | Discussion-focused | Earlier broad WIP exposed S1AP private context and proxied through message type; superseded by the focused merged !1019 semantic discriminator. |
| !1016 | merged | Scanned | GitHub Actions stable-branch update; discussion catches an accidental wrong target branch. |
| !1015 | merged | Scanned | Adds NAS 5GS Release 16 UPDP messages. |
| !1014 | merged | Scanned | master-3.2 backport correcting a three-bit NAS request-type mask. |
| !1013 | merged | Scanned | release-3.4 backport correcting the same NAS request-type mask. |
| !1012 | merged | Scanned | Master correction of the NAS request-type mask from four bits to three. |
| !1011 | merged | Scanned | Qt 5.15 deprecation cleanup keeps version guards/fallbacks for older supported Qt. |
| !1010 | merged | Scanned | NAS 5GS v16.6.0 IE update; protocol-specific. |

## Durable synthesis

- Prefer TVBuff transformation/alignment and encoding helpers over hand-built scratch buffers when the common API directly describes the wire transformation (!1059, !1042, !1035).
- A build-tool dependency can be removed from master only after its final master consumer disappears, while shared build/support infrastructure may still need it for maintained release branches (!1055; Guy Harris review).
- When a reusable crypto context has reset semantics, initialize/open it once, reset/rekey per iteration, and clean it up once rather than repeatedly destroy/recreate it (!1047, !1049, !1052, !1054).
- Prefer field-registration display policy such as `BASE_HEX_DEC` over hand-built spacing/format strings, and expose captured padding/alignment bytes when they are real wire bytes (!1048, !1037).
- Bootstrap feature enablement is incomplete until stale dependency artifacts are invalidated/rebuilt as needed and the resulting binary is tested for the requested capability (!1039).
- Never encode a historical platform-version string shape as a permanent parser assumption; compare the semantic version fields needed by the policy (!1038, Guy Harris).
- Validate cheap framing invariants as soon as their controlling field is decoded, and preserve structured error information across helper boundaries (!1029, !1027, Guy Harris).
- Match platform error retrieval to the underlying object/API family; sockets and pipes need not share an error domain (!1030, Guy Harris).
- For generated nested dissectors, carry the minimal semantic discriminator actually needed to select decoding; do not expose broad private state merely to proxy an outer message category (!1017 → merged !1019).
- Security-oriented I/O refactors must respect distinct file/pipe/socket semantics and should first question whether the operation needs to occur while privileged at all (!1045, authoritative Guy Harris review; implementation unmerged).
