# Durable conventions from MRs !158-!207

This batch reinforces several existing notebook themes and adds several clear early examples.

## Wire decoding and display are separate contracts

Merged master !177 received detailed Pascal Quantin review after a proposed MBIM version field used the wrong byte order because it happened to produce the desired current display. Pascal required the specified little-endian decoding and a separate display transformation for the implied decimal notation, explicitly noting that the shortcut would fail for future values. Decode the actual wire representation first; perform notation, units, and range formatting separately.

Merged master !203 makes the 64-bit `BASE_SPECIAL_VALS` rendering path match the established 32-bit behavior: named special values use the table text, while unmatched values remain ordinary numeric values rather than becoming “Unknown.”

Merged master !169 fixes QUIC Key Phase presentation because a Boolean value had been normalized to 0/1 before being passed through a field registration whose mask expected the bit in its original position. Registered masks and manually supplied values must agree on their input domain.

## Review sibling implementations when a duplicated parser bug is found

In merged !202, Pascal Quantin noticed that the same wrong spare-bit offset existed in all three related GSM RR Channel Description decoders and asked for all of them to be corrected. When code or protocol structures are duplicated, a demonstrated bug should trigger a deliberate search of sibling implementations rather than a one-site repair.

## Structured-output changes require migration-quality documentation

Merged master !184 deliberately replaced the Follow Stream YAML schema. Review accepted the compatibility break on the development branch only after requiring release-note coverage, user-guide documentation, and an explicit explanation of how the old `peerN_M` keys map to the new peer/index representation. Machine-readable output is an interface even when it can evolve.

## Platform-dependent integer types need portable formatting

The first merged version of !184 exposed a Windows and then macOS formatting failure around `time_t`: a C format that matched one ABI did not match another. Review converged on converting to a known-width type before using the corresponding portable format macro. Do not infer platform typedef widths from one compiler.

## Windows console attachment must preserve redirected streams

Merged !180 moved console and logging initialization earlier, but subsequent discussion found that attaching to a parent console could disrupt an inherited stdin pipe used by capture-from-stdin. The proposed correction preserves the existing standard-input handle when input is not supposed to be redirected. Platform bootstrap code must treat inherited pipes as live resources, not replace them merely because a console is being attached.

## Reviewability constrains submission size

During merged !166, Pascal Quantin noted that a very large `packet-mq.c` diff exceeded GitLab's render limit and could not be meaningfully reviewed. The contributor reduced the submitted delta and deferred additional work. A change should be split or narrowed when the review system cannot display its substantive diff.

The same MR also demonstrates that a successful “up to date” rebase is meaningless if the remote named upstream actually points at the contributor's fork. Verify repository remotes before treating rebase status as evidence.

## CI jobs should match runner availability

Merged !161, authored by Gerald Combs, limits the Windows merge-request build to the canonical project because it depends on a project-specific Windows runner and custom image unavailable to ordinary forks. Project-specific infrastructure should be represented explicitly in CI job scope rather than making contributor pipelines fail for environmental reasons.

## Submission tooling follows the current review system

In merged master !158, Pascal Quantin asked the contributor to refresh the repository's current commit-message hook and remove obsolete Gerrit `Change-Id` trailers now that Wireshark used GitLab. Submission metadata and local hooks should match the review system that actually consumes them.

## String field type follows termination and padding semantics

Guy Harris's merged !207, !205, !204, and !191 supply early examples behind the later, stronger string-field taxonomy already recorded in the notebook. BPDU and AFP fields with defined fixed-width NUL padding use padded-string semantics; Aeron's non-NUL-terminated error string is an ordinary string. !207's SAP interpretation is historical and must be read through the later Guy Harris work that introduced distinct truncated-string semantics for undefined bytes following an early NUL.
