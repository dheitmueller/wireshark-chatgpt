# Durable conventions from !1010-!1059

This file captures the cross-cutting rules promoted from the run. Current upstream source and project documentation remain authoritative.

## Prefer TVBuff transformations over manual scratch-buffer reconstruction

Merged master !1059, authored by John Thacker, replaces a manual `tvb_get_ptr()` plus nibble-shift loop and scratch `tvb_new_real_data()` with `tvb_new_octet_aligned()`. Merged !1042 similarly replaces a local GSM TBCD conversion loop with `tvb_get_string_enc()`, while !1035 adds a shared Base64-to-child-TVBuff helper.

**Rule:** when a TVBuff/encoding helper directly expresses the wire transformation, prefer it to raw pointers, local copy loops, and hand-created scratch TVBs. The common helper centralizes bounds, lifetime, and representation semantics.

## Remove build dependencies at the consumer boundary, while respecting maintained branches

Merged !1055 converts Wireshark's last Bison/YACC grammar to Lemon and removes the now-unused master build/package/documentation dependency. Guy Harris explicitly notes that shared support images cannot immediately drop Bison because maintained 3.4/3.2 branches still require it.

**Rule:** removing the final consumer on one branch justifies removing that branch's build dependency, but shared CI/bootstrap images must be evaluated against every maintained branch they serve. Treat cross-repository support images as part of the compatibility matrix.

## Reuse resettable crypto contexts instead of destroy/recreate loops

Merged !1047, !1049, !1052, and !1054 initialize/open libgcrypt digest or cipher state outside repeated loops, use the library's reset/rekey operation for each iteration, and clean up once. Besides reducing allocation churn, the changes remove lifecycle patterns reported by Coverity as double frees.

**Rule:** when the dependency documents reset semantics that restore the context to the required reusable state, make ownership span the loop and reset between independent operations. Do not mechanically apply this to APIs whose reset contract does not restore all required state.

## Prefer declarative field display policy, and make real wire padding visible

In merged !1048, Anders Broman, Alexis La Goutte, and Jaap Keuter steer MQ presentation toward `BASE_HEX_DEC` / `BASE_DEC_HEX` and away from bespoke spacing intended to make the tree look like a table. In merged !1037, Anders and Alexis require XDR alignment bytes to be represented as a Padding field instead of silently skipped.

**Rule:** use field-registration display flags for normal numeric rendering whenever possible so presentation stays consistent across protocols. Bytes physically present in the captured encoding, including alignment/padding, should generally remain visible in the tree when showing them makes offsets and framing auditable.

## Feature-enabling bootstrap changes must verify capability and handle stale builds

Merged !1039 adds PKCS #11 support to the macOS support-library build. John Thacker provides both feature-specific tests and `tshark -v` capability verification; Jörg Mayer finds that a previously built GnuTLS must be removed/rebuilt before the newly enabled capability appears.

**Rule:** a bootstrap-script change is not validated merely because dependencies build successfully. Verify the resulting Wireshark binary exposes the requested capability and make upgrade/invalidation behavior explicit for cached dependency builds that were configured before the feature was enabled.

## Parse platform versions semantically, not by historical era

Merged master !1038 is authored and merged by Guy Harris. The old macOS setup logic assumed every target/SDK version was `10.N`; Big Sur made that false. The accepted code parses major/minor components and compares them numerically.

**Rule:** version policy should compare the semantic components it depends on rather than strip a historically constant prefix or assume a permanent vendor numbering era.

## Validate framing invariants early and preserve error identity

Merged master !1029, authored and merged by Guy Harris, checks pcapng `block_total_length` for the required 4-byte multiple immediately after the length is read and reports the actual invalid value. Merged master !1027, also authored and merged by Guy, propagates a numeric write error through a helper so its caller can report the real failure.

**Rule:** check cheap structural properties at the point their controlling field becomes available, before allocation or deeper dispatch. When a lower layer has a structured error code, carry that code across helper boundaries instead of collapsing it to a Boolean that forces the caller to invent a generic diagnosis.

## Match error APIs to the underlying handle domain

Merged master !1030, authored and merged by Guy Harris, limits `WSAGetLastError()` to socket I/O; the pipe path uses the ordinary errno domain.

**Rule:** select error retrieval and formatting according to the API that performed the failed operation, not merely the host OS. A platform can expose multiple independent error domains for files, pipes, sockets, or other native handles.

## Carry the semantic discriminator a nested generated decoder actually needs

Closed WIP !1017 attempted to expose broader S1AP private state so NGAP could influence S1AP decoding. Merged master !1019 instead records a focused `transparent_container_type` at the ASN.1/conformance layer and switches nested container decoding on source-to-target versus target-to-source semantics; the generated C is regenerated from those authoritative inputs.

**Rule:** pass or record the narrow semantic property that controls nested decoding rather than exposing a large private context or proxying through a broader outer-message classification. For generated dissectors, make the change in the generator inputs/conformance/template layer and regenerate.

## Treat TOCTOU fixes as object-model and privilege-boundary reviews

Closed !1045 proposed replacing a pre-open `stat()` classification with post-open `fstat()`. Guy Harris points out that `open()` is not guaranteed to work on Unix-domain sockets and, more fundamentally, questions why this pipe/socket path should be doing privileged file operations at all.

**Rule:** a race/security cleanup is not complete if it assumes files, FIFOs, and sockets share the same open/classification behavior. Start by minimizing the privileged region, then design classification/open/connect flow around each supported object type. This is authoritative review guidance from a closed MR, not accepted implementation precedent.
