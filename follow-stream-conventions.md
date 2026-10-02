# Wireshark Follow Stream Conventions

This file records durable conventions for Follow Stream and tap design. Current upstream source remains authoritative.

## Carry logical substream identity when one packet can contain several streams

Merged master MR !3628, authored by Ivan Nardi, fixes HTTP/2 and QUIC Follow Stream behavior for packets that contain data from several logical streams. The former path effectively assumed that all followable data in one physical packet belonged to the selected stream. The accepted design adds logical `stream_id` to protocol follow-tap data, records the selected substream in `follow_info_t`, and lets HTTP/2/QUIC-specific tap listeners discard data from other logical streams before handing the TVB to the generic follow machinery.

The MR also adds dedicated HTTP/2 and QUIC multistream capture fixtures plus automated tests selecting a specific stream.

**Architecture rule:** capture-record identity is not sufficient when a multiplexed protocol can carry several logical streams in one packet. Carry the logical stream/substream identifier through the tap contract and filter where the protocol still has authoritative stream identity.

**Testing rule:** include a capture in which at least two logical streams share a packet and verify that following one stream returns only that stream's bytes. Test both CLI and shared follow infrastructure behavior when practical.

**Confidence:** Very high. Merged master implementation with focused regression captures/tests and explicit rationale in the MR description.


## Follow-stream state is capture-scoped, and tap the payload before child dispatch

A Follow Stream implementation normally assigns stream identities that are meaningful only within the current capture and taps the transport payload before conversation or heuristic subdissectors can consume/transform it.

Merged master MR !1854 adds Follow DCCP Stream support. Pascal Quantin required the global DCCP stream counter to be reset through `register_init_routine()` when a new capture is opened. He also moved the follow tap to immediately after creation of the payload TVB, before conversation/heuristic subdissector dispatch. The same review rejected carrying unrelated Exported PDU plumbing in the MR and required the supplied capture to be exercised by an actual test suite rather than merely checked into `test/captures`.

**State rule:** counters/registries that assign Follow Stream IDs are file/capture scoped. Register an init/reset routine so reopening or switching captures cannot leak numbering or state.

**Tap-placement rule:** when Follow Stream is defined over the transport payload, queue that payload before child protocol dispatch unless the protocol's contract explicitly says otherwise.

**Testing rule:** a new capture fixture has value only if a regression test consumes it. Add focused Follow Stream assertions for the new transport rather than landing an otherwise-unused sample.

**Submission rule:** keep adjacent but unrelated plumbing out of the feature MR; Pascal explicitly asked for Exported PDU cleanup to be separated.

**Confidence:** Very high. Merged master feature with extensive direct Pascal Quantin review and a representative automated test.
