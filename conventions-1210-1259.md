# Wireshark conventions from MRs 1210-1259

These rules supplement the topical notebook files. Current upstream source remains authoritative.

## Do not use Boolean truthiness as a validity test for an enum whose zero value is valid

Merged master MR 1243 fixes SNMPv3 authentication when MD5 is selected. MD5 is authentication model value zero, so testing the model value as a Boolean incorrectly treated a valid configured model as absent. Merged stable backports 1258 and 1259 carry the same correction.

**Rule:** separate presence or validity from the selected enum value. Use an explicit presence predicate for whether state exists and named comparisons for the enum itself. Numeric zero is not absence unless the protocol or API contract explicitly defines it that way.

**Confidence:** Very high. Merged master correctness fix plus two merged stable backports.

## For generated ASN.1 dissectors, edit the authoritative input and regenerate through the supported target

During merged MR 1243, Jaap Keuter pointed out that packet-snmp.c is generated. Guy Harris then specified the accepted workflow: edit the ASN.1 configuration source, invoke the project's ASN.1 regeneration target, and include both the authoritative input and regenerated output in the merge request.

**Rule:** do not repair generated dissector output directly. Change the generator-side ASN.1, configuration, or template source, run the supported build target, and submit the regenerated artifact alongside it.

**Confidence:** Extremely high. Direct Guy Harris and Jaap Keuter guidance on a merged master fix.

## Give independent owning wrappers independent OS resources

Merged master MR 1240 fixes a Windows crash in the Qt endpoint-map path by duplicating a file descriptor before wrapping it in a C stdio stream. The Qt file object and the FILE stream can then close their own descriptors without double-owning the same underlying descriptor. Stable MR 1242 backports the same behavior.

**Rule:** when bridging ownership domains such as Qt and stdio, identify which layer closes the resource. If two wrappers need independent close lifecycles, duplicate the descriptor or handle before the second ownership transfer.

**Confidence:** Very high. Merged master fix with a merged stable backport.

## Preserve temporary-directory placement when supplying a custom temporary filename

In merged MR 1223, Guy Harris cited the Qt QTemporaryFile contract: the default constructor chooses the temporary directory, but a custom relative template is relative to the current working directory. The accepted code explicitly combines the desired template with QDir::tempPath(). Stable MR 1237 carries the same correction.

**Rule:** do not assume "temporary lifetime" implies "temporary directory placement" after supplying a custom filename. Use the API's default temporary placement or explicitly anchor the custom template in the platform temporary directory.

**Confidence:** Extremely high. Merged fix with direct Guy Harris documentation-based review.

## Resolve stable dissector identities during handoff, not in the packet hot path

Merged MR 1229 moves O-RAN dissector lookup out of the eCPRI packet path and caches the handle in handoff. Merged MR 1231 does the same for UDPCP's XML subdissector. In the superseded O-RAN MR 1210, Jaap Keuter explicitly required the stable lookup to be done once in the handoff routine.

**Rule:** if a named dissector identity is stable after registration, resolve it once in handoff and retain the handle. Do not repeatedly search the dissector registry for every packet.

**Confidence:** Very high. Two merged implementations plus direct maintainer review in the superseded precursor.

## Validate packet-derived arithmetic invariants before division and stop malformed decode paths

Merged O-RAN MR 1222 later triggered a Coverity divide-by-zero warning in the beamforming-weight path. Merged MR 1245 validates IQ width and weight count before division, reports malformed values with Expert Info, and stops the unsafe decode path. Martin Mathieson then fuzz-tested a capture exercising that extension.

**Rule:** validate divisors, strides, widths, and counts before they become arithmetic operands. When invalid packet data makes further interpretation unsafe, report it and stop that decode path rather than computing through it. Follow static-analysis findings with a representative regression or fuzz input when practical.

**Confidence:** Very high. Merged fix following a concrete static-analysis finding and representative fuzz validation.

## Distinguish merge-train CI context from contributor-commit context

Merged Gerald Combs MR 1234 makes the commit validator skip GitLab merge-train jobs because the train commit is platform-synthesized rather than an ordinary contributor commit. Stable MRs 1238 and 1239 carry the same behavior.

**Rule:** define which Git object a metadata validator is intended to check. In merge-train context, either resolve the underlying submitted commits explicitly or skip validation that only makes sense for ordinary contributor commits.

**Confidence:** Very high. Merged Gerald Combs CI change with two stable backports.

## Express platform-only source narrowly enough that cross-platform checkers can still run

Merged MR 1228 wraps a Win32-only translation unit in a Win32 guard so non-Windows Clang validation does not require Windows headers. Stable MRs 1232 and 1233 carry the same solution.

**Rule:** when a source file genuinely has no meaning off-platform, make that platform contract explicit at the narrowest practical boundary rather than disabling the checker broadly. A whole-file platform guard is appropriate when the entire translation unit is platform-specific.

**Confidence:** Very high. Merged master change with stable backports.

## Keep merge requests focused and make the subject say what changed

During review of merged MR 1231, Guy Harris objected that it combined two separate changes and that the title did not describe the actual change.

**Rule:** one merge request should normally carry one coherent change. Use a component-specific subject that says what changed, rather than a vague title or an unrelated collection of cleanups.

**Confidence:** Extremely high for review practice. Direct Guy Harris feedback on a merged MR.

## Make broad fallback dispatch opt-in when it changes semantics or packet-path cost

Merged DCT2000 MR 1216 allows protocol-name-to-dissector fallback only behind a preference that defaults off. Martin Mathieson, the original dissector author, was concerned about many failed lookups on large logs. Pascal Quantin and Guy Harris also pointed out that Wireshark already has an Exported PDU mechanism for arbitrary upper-layer payloads.

**Rule:** a convenience fallback that broadens dispatch semantics or adds potentially expensive lookup work to common traffic should not silently become the default. Prefer the native abstraction when one exists; otherwise make the behavior explicitly configurable and conservative by default.

**Confidence:** High. Merged implementation plus substantive review from Martin Mathieson, Pascal Quantin, and Guy Harris.

## Bootstrap scripts must follow each dependency's actual build and linkage contract

Guy Harris's merged MRs 1248 through 1251 show that dependency cleanup and linkage cannot assume one universal build-system model. Snappy's CMake build lacked uninstall and distclean targets, so Wireshark explicitly removes installed artifacts and the build directory. The macOS build also selects a shared Snappy library so its C++ runtime dependency is carried correctly when linked from libwireshark.

**Rule:** encode the dependency's actual install, uninstall, cleanup, and runtime-linkage behavior. Do not assume every dependency exposes autotools-style targets or that static and shared variants impose the same language-runtime requirements.

**Confidence:** Extremely high. Consecutive merged changes authored by Guy Harris.
