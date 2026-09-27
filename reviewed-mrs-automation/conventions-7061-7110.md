# Conventions from !7061–!7110

- !7064, !7100, !7101, !7103–!7106: Conversation lookup wildcard flags and conversation-creation endpoint-omission flags are distinct API domains. Audit existing callers when the core API contract changes.
- !7089, !7108: Normalize nullable conversation addresses at the API boundary and let internal code rely on the documented normalized invariant.
- !7102: A nested protocol element must fit both the captured TVB and the enclosing declared option/IE region.
- !7099: Indexed subtree arrays must be large enough for every possible index and must be registered before use. Stig Bjørlykke identified both defects after merge.
- !7088, !7079: UAT and cache cleanup callbacks must free all owned nested allocations and leave surviving record pointers in a safe state.
- !7085: Dynamic field registration should reflect actually enabled/configured features; large numbers of unnecessary fields can make profile reloads very expensive.
- !7081: Use the GLib integer/pointer conversion helpers for 32-bit integer values carried through gpointer-based APIs.
- !7070: Account for C integer promotion when reasoning about arithmetic and wrap checks on narrow unsigned types.
- !7077: Maintained targets disabled in the default build still need explicit CI build coverage.
- !7076: Distinguish compiled capability, runtime platform support, and state that is only known after subsystem initialization.
- !7097, !7099: Protocol additions should provide representative captures; review should also exercise unknown/default paths and structural registration.
- !7072: Prefer standard CMake user presets for local developer configuration when they replace a project-specific mechanism.
