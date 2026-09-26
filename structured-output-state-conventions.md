# Wireshark Structured-Output State Conventions

This file records durable conventions for stateful structured-output helpers used by dissectors. Current upstream helper APIs remain authoritative.

## Empty semantic values still require their structural transitions

For a stateful emitter, begin/end transitions describe the semantic structure, not whether a payload contains one or more bytes. A zero-length value can still require both transitions to leave the emitter in a valid state.

Merged master MR !9126, authored by John Thacker, fixes Protobuf JSON mapping for a zero-length bytes field. Once the field name had been emitted, the JSON dumper still needed to enter and leave the base64 value state even when there were no bytes to write. The old code put the begin/end calls inside the non-empty-buffer branch, so the dumper state became inconsistent for an empty value. Release-4.0 MR !9130 carries the same correction.

The master change also creates the JSON dumper only when JSON mapping output is actually requested, avoiding construction and work for a disabled derived-output path.

**Implementation rule:** drive structured-output state from the presence of semantic elements, not from payload length. Keep begin/end operations balanced for empty arrays, strings, byte sequences, or other zero-length values when the format still contains that element.

**Testing rule:** stateful serializer/dumper tests should include zero-length values in every value class whose output path has begin/write/end phases. Non-empty test data cannot expose missing structural transitions.

**Confidence:** Very high. Merged master correctness fix by John Thacker with an accepted stable backport.
