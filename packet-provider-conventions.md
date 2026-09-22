# Wireshark Packet Provider Conventions

This file records durable conventions for packet-provider callbacks and shared capture-file semantics. Current upstream source remains authoritative.

## Centralize shared provider semantics instead of maintaining frontend-specific copies

When multiple Wireshark frontends or execution paths implement the same packet-provider callback, nearly identical local copies are a correctness risk rather than harmless duplication. Provider behavior is part of the semantic contract presented to epan; all consumers should resolve the same frame and metadata cases in the same order.

Merged master MR !13729, authored and merged by Guy Harris, replaces several almost-identical frame-timestamp provider functions with the shared `cap_file_provider_get_frame_ts()`. The common helper checks the reference frame, previous dissected/captured frames, and then the frame sequence, and only returns an absolute timestamp when the selected frame actually has one. Centralizing the callback removes the opportunity for one frontend to omit a provider state such as the reference frame or to drift as the lookup rules evolve.

**Architecture rule:** if a provider callback expresses capture-file semantics rather than frontend policy, implement it once at the capture-file/provider layer and reuse it from GUI, command-line, and other epan consumers. Keep frontend-local callbacks only when the semantic policy genuinely differs.

**Review rule:** when two provider callbacks look nearly identical, compare not just their current code but every state source they consult and the lifetime/validity rules of their return values. Treat small differences as possible semantic drift that should either be intentional and documented or eliminated.

**Testing rule:** exercise provider behavior through more than one frontend/path, including reference-frame and cached previous-frame cases rather than only ordinary frame-sequence lookup.

**Confidence:** Extremely high. Merged master architectural cleanup authored and merged by Guy Harris, directly consolidating duplicated provider semantics into the shared capture-file layer.
