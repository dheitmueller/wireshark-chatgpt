# Wireshark CI Build-Matrix Conventions

This file records durable review and testing rules for platform- and feature-specific build configurations. Current upstream CI and platform documentation remain authoritative.

## A green general CI pipeline does not prove unexercised platform combinations still build

Wireshark's supported build matrix includes platform-, package-, and optional-feature-specific paths that cannot all be exercised in every merge pipeline. Changes to broadly included headers, shared build plumbing, or generic utility definitions can therefore break configurations that the normal pipeline never compiled.

Merged master MR !15730 fixes a Windows `ws80211_utils` build broken by the preceding `array_length()` cleanup in !15727. The discussion is unusually explicit: the Fedora MinGW pipeline had passed, but that configuration did not include AirPcap, and the same broad header change had recently broken the Falco bridge and Debian packaging as well. John Thacker noted that the number of platform/configuration combinations makes it impractical to build everything in ordinary CI because of CI-minute limits.

**Review rule:** for changes to common headers, foundational utility macros, dependency includes, generated build inputs, or other code with a very broad compile surface, identify supported configurations that the ordinary pipeline does not exercise. Run targeted jobs/builds where practical and audit known conditional consumers instead of treating one green cross-build as proof of universal coverage.

**Submission rule:** if a large mechanical refactor moves a widely used definition to a new header or otherwise changes transitive include dependencies, call that dependency-shape change out separately from the textual replacements. Build failures in optional/platform-specific consumers are not evidence that the mechanical source transformation itself was wrong, but they are part of the refactor's compatibility surface and should be anticipated.

**Confidence:** Very high. Merged master repair approved and merged by John Thacker, with direct discussion of multiple real breakages and the limits of the normal CI matrix.

## Treat conditional library builds and alternate parser entry paths as required compatibility tests

**Merged master !9372** adds calculated HTTP/2 connection/stream flow-control windows but illustrates the limits of a successful default pipeline. **Alexis La Goutte** requested a build with `HAVE_NGHTTP2` disabled because the patch had conditional-compilation mistakes and relayed a Clang Analyzer null dereference. **John Thacker** later reported regression #18815: `dissect_http2_settings_ext` can receive a null `http2_session_t`, a path the new implementation did not handle. The author acknowledged the omission and filed follow-up !9534. Thus the merged feature is **not** evidence that its original initialization was correct.

**Testing rule:** for changes to an optionally compiled protocol component, compile with the dependency both present and absent. Trace alternate dissector entry points, malformed/partial captures, and null context arguments; add a regression case for the failing path even when the primary flow works. **Confidence:** very high for the explicit Alexis/John feedback and observed post-merge regression.

## Verify new dissectors with MSVC narrowing checks as well as analyzer warnings

Merged ATSC LLS !9355 demonstrates two independent review gates: Alexis La Goutte asked the contributor to address Clang Analyzer dead stores and add the new dissector to release notes. Chuck Craft then supplied a Windows C4244 warning-as-error trace where a guint16 variable, actually obtained from a one-byte field, was passed to a guint8 parser argument. The author identified follow-up !9375 for the Windows correction, so the initially merged MR should not be treated as complete Windows validation. A separate WSLua repair in merged !9321 likewise corrects an MSVC narrowing warning on guint64-to-lua_Number default arguments after the unmerged !9323 draft.

**Rule:** review implicit narrowing at parser API boundaries and configure supported Windows warning-as-error builds, even when the originating platform/compiler passes. Treat cross-platform post-merge repairs as evidence gaps in the initial test matrix, not failures to be hidden by broad warning suppression. **Confidence:** high (maintainer review, concrete MSVC diagnostics, subsequent merged repairs).
