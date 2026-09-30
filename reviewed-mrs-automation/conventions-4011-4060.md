# Durable conventions from !4011–!4060

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Separate heuristic recognition from explicit Decode As dissection

Merged !4059 was revised after John Thacker pointed out that Decode As should not behave like the heuristic recognizer. The heuristic may reject a packet when DVB-S2 CRC/signature checks fail; once the user explicitly forces DVB-S2 through Decode As, the normal dissector should continue far enough to expose the structure and report the bad CRC rather than returning zero.

**Rule:** use strict plausibility checks to decide whether a heuristic may claim ambiguous traffic. Keep those ownership checks out of the forced/explicit dispatch path unless they are true parser preconditions. Explicit Decode As is a user assertion of protocol ownership, not another heuristic probe.

## Instantiate composite TVBs only after the first component exists

Merged !4057, authored by John Thacker, fixes an assertion reached on false-positive DVB-S2 heuristic matches because an empty composite TVB had been created even though no child TVB would be appended.

**Rule:** a composite TVB represents one or more component TVBs. Create it lazily when the first component is actually available; do not use an empty composite as a placeholder for “possibly later.”

## Keep file-format serialization semantics out of generic Wiretap option APIs

Merged !4052, authored by Guy Harris, removes generic option-size helpers whose definition of “size” was actually the padded serialized size in a pcapng file. pcapng now computes and writes its options in the pcapng writer using the same local machinery as other pcapng blocks.

**Rule:** Wiretap’s generic block/option API should model semantic values and ownership independent of any one file format. Padded lengths, on-disk option headers, and other serialization details belong to the corresponding reader/writer implementation.

## Cross-build generators need host compilation and target-derived executable paths

Merged !4048 is the master-origin Lemon cross-compilation fix later seen through stable !4466/!4467. Anders Broman’s Windows test exposed configuration-dependent executable locations, and Gerald Combs recommended invoking `$<TARGET_FILE:lemon>`. The MR also permits Lemon to use a build-host compiler during a target cross-build.

**Rule:** classify build-time generators as host tools. Compile them for the build host and invoke the concrete CMake target path rather than assuming the program is on PATH or reconstructing a platform/configuration-specific output directory.

## Make the common Wiretap read layer establish record-block ownership

Merged !4042, authored by Guy Harris, ensures that every successful record read has a block, and that a block allocated for a failed read is released by the common path. Readers no longer independently decide whether ordinary records happen to have metadata containers.

**Rule:** when a resource/invariant is universal to all records, establish and unwind it at the common abstraction boundary. Downstream code should not need a third “valid record but no block” branch merely because individual file readers historically allocated metadata inconsistently.

## Fix event/race causality rather than papering over stale callbacks

Closed !4036 proposed broad NULL guards around IO Graph callbacks. Guy Harris explicitly rejected masking race behavior; Roland Knall traced the crash to redundant UAT model `dataChanged` emissions and fixed the causal model-update behavior in merged !4135.

**Rule:** a guard that merely turns a stale/racing callback into a no-op is not a substitute for correcting the lifecycle or notification ordering that made the callback possible. Prefer repairing the owning model/event contract, then retain defensive checks only where the state is legitimately optional.

## Preserve outer TCP desegmentation state across nested dispatch

Merged stable MRs !4013 and !4012 restore `pinfo->can_desegment` from `pinfo->saved_can_desegment` before AMQP dispatches into a version subdissector that will consume desegmentation capability again.

**Rule:** nested dissectors share `packet_info` desegmentation state. Before handing off to a path that applies its own desegmentation bookkeeping, restore or preserve the caller’s saved capability exactly; do not allow nested decrements to accidentally disable the enclosing TCP PDU engine.

## Treat command-line preferences as a precedence layer with an explicit supersession point

Merged !4050 keeps `-o` preference assignments alive when Lua plugins are reloaded and preference registrations are recreated. If the user later edits that preference in the GUI, the saved command-line assignment is dropped so another reload does not undo the user’s newer choice.

**Rule:** when configuration sources have precedence and subsystems can be reloaded, preserve the effective source across reconstruction but record when a higher-level user action intentionally supersedes it. Reload should recreate current state, not resurrect stale initialization input.
