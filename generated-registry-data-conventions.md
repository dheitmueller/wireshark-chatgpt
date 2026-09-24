# Wireshark Generated Registry Data Conventions

This file records durable conventions for shipped registry datasets that can be transformed into generated code or tables. Current upstream generators, update jobs, and runtime lookup behavior remain authoritative.

## Precompile stable global registry data without eliminating user override layers

Large shipped registries that are stable for the lifetime of a build need not be reparsed from text on every Wireshark or TShark startup. If the global dataset is produced by an existing update workflow, the update step can generate a lookup representation optimized for runtime use while retaining the human/distribution artifact where it remains part of the project contract.

Merged master MR !11315, authored and merged by Martin Mathieson, replaced runtime parsing of the global `enterprises` registry with generated C data after profiling showed that loading the text file accounted for about 10.9% of a short-capture TShark run. The accepted design keeps the generated global entries immutable while preserving profile/personal files as the runtime customization layer. Gerald Combs confirmed that the generator is already part of Wireshark's scheduled assigned-number update workflow and explicitly noted that analogous generated data such as `manuf` can also be consumed externally, so changing runtime representation must not accidentally discard an externally useful published artifact.

Merged master MR !11358, authored and merged by João Valverde, independently applies the same idea to the services registry: generate sorted static data for the shipped baseline and perform efficient lookup instead of reparsing a large global text file at startup, while retaining the user's personal services data as the mutable override source.

**Implementation rule:** distinguish the shipped authoritative baseline from runtime user configuration. It is appropriate to compile or generate a stable global registry into static arrays, binary-search tables, perfect hashes, or similar structures when that materially reduces startup work, but preserve the documented personal/profile override semantics rather than making the optimized global representation the only source of truth.

**Update-workflow rule:** make the generated runtime representation a deterministic product of the canonical registry-update job. Keep generator names, paths, invocation assumptions, and scheduled update automation synchronized. If the source text or another derivative is itself a supported distribution/public artifact, continue generating or publishing it even when Wireshark no longer parses it at runtime.

**Review rule:** performance changes to registry loading should verify both lookup equivalence and precedence behavior. Test a shipped/global value, a personal/profile override of that value, and values present only in either layer. Also check packaging and external-consumer expectations before deleting a formerly runtime-read data file.

**Confidence:** Very high. Two merged master changes independently move major global registries to generated static lookup structures, with explicit startup-performance motivation and senior-maintainer discussion of scheduled regeneration, personal overrides, and external artifact use.