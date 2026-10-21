# Key findings: 1360-1409

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

The exact accounting is in ledger-1360-1409.md. Merged changes were weighted above closed work, with Guy Harris-authored portability and build changes treated as especially authoritative.

- !1404, by Guy Harris, replaces OS-name assumptions about high-resolution clock APIs with an explicit capability check and fallback. !1407 is the release-3.4 backport. The rationale distinguishes OS families from the particular OS, C library, and compiler versions that expose an API.
- !1392 and !1367, also by Guy Harris, show that a dependency version does not by itself prove an optional feature: GnuTLS can meet the version floor yet be built without PKCS #11 support, so the build checks the actual capability.
- !1400 adds the typed-item and true/false-string checkers to Ubuntu merge-request CI as report artifacts. Martin Mathieson explains that immediate hard failure would be premature while substantial pre-existing checker findings remained. This is useful staged-checker-rollout precedent.
- !1396 is direct checker-driven field-metadata evidence: integer registrations are widened to match the byte lengths and masks actually used by protocol-tree APIs.
- !1388 fixes WSLua behavior for generated fields that legitimately have no backing TVB. The accepted API distinguishes semantic absence from an expired source object, and FieldInfo.range returns nil when no source range exists.
- !1364 hardens QUIC heuristic recognition with several structural invariants and supplied negative examples. Persistent conversation state is created only after recognition succeeds. !1386 is the stable backport.
- !1360 fixes seven SOME/IP-SD generated or hidden fields whose byte ranges were displaced by one full entry. The correction preserves the original entry offset separately from the advancing parse cursor.
- !1401 changes the generated MPTCP subflow summary from label-only presentation to an actual string field value so Apply as Column receives the information shown in the tree.
- !1393 fixes an IDN bounds error by checking the index before decrementing it while walking alignment padding.
- The Guy Harris macOS setup sequence in this batch repeatedly treats build-system behavior, runtime linkage, optional dependency features, install/uninstall behavior, and platform-tool differences as explicit contracts rather than relying on defaults.
- !1398 validates the fixed two-byte HE 6 GHz capabilities structure before decoding its bitmask, corroborating existing fixed-length and native bitmask practices.
- !1362 and !1387 add GQUIC legacy-version encapsulation by handing a bounded child TVB to the QUIC dissector; the master change includes a sample capture.

Closed and down-weighted: !1409, !1403, !1390. They are broad mechanical allocation-style migrations and do not outweigh later merged allocation-policy evidence already in the notebook.

No SMPTE ST 291/VANC packet type was encountered.
