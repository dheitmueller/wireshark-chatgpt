# Wireshark Registry Metadata Conventions

This file records durable conventions for registries and duplicated metadata used for capture-file and protocol dispatch. Current upstream source remains authoritative.

## Keep one authoritative owner for metadata that affects dispatch or user-visible capabilities

Duplicating the same descriptive or dispatch-related fact across multiple tables creates synchronization work without improving correctness. If one copy is not actually consumed, remove it rather than preserving an apparently useful mirror that can silently drift. If several tables really are required, their distinct responsibilities and ordering semantics should be explicit.

Merged master MR !11899, authored by Guy Harris, removes per-open-routine extension lists for magic-number capture readers because those lists were unused and constituted another place where file-type extension metadata could become inconsistent. Guy also called out the larger architectural problem: Wiretap had multiple tables describing capture-file types, while `open_info` ordering required careful manual placement of weak heuristics so they ran only after stronger recognizers.

The surrounding Guy Harris changes !11880, !11881, !11896, and !11897 expand and correct comments explaining those tables and their extension semantics, reinforcing that the tables have different roles rather than being interchangeable copies of the same registry.

**Implementation rule:** do not add a second metadata field or registry merely to mirror information already owned elsewhere. First identify which component consumes the information and make that component's representation authoritative. Remove unused mirrors; where separate registries have genuinely different roles, document the ownership boundary and derive shared facts when practical.

**Dispatch rule:** recognition precedence is part of the file-format detection contract. Weak heuristic readers must not accidentally outrank stronger magic/signature-based readers simply because of registration order. Keep precedence policy centralized or otherwise explicit enough that adding a reader cannot silently perturb unrelated detection.

**Review rule:** when changing capture-file registration metadata, audit all tables and UI consumers that expose the same concept (extensions, type names, readers/writers, open routines), but do not assume they all should store the same copy. Ask which table is authoritative, which data are derived, and whether ordering itself carries semantics.

**Confidence:** Extremely high. The core cleanup was authored and merged by Guy Harris, with a surrounding series of accepted documentation/cleanup changes explaining the same architecture.