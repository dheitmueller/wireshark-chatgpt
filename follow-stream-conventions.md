# Wireshark Follow Stream Conventions

This file records durable conventions for Follow Stream and tap design. Current upstream source remains authoritative.

## Carry logical substream identity when one packet can contain several streams

Merged master MR !3628, authored by Ivan Nardi, fixes HTTP/2 and QUIC Follow Stream behavior for packets that contain data from several logical streams. The former path effectively assumed that all followable data in one physical packet belonged to the selected stream. The accepted design adds logical `stream_id` to protocol follow-tap data, records the selected substream in `follow_info_t`, and lets HTTP/2/QUIC-specific tap listeners discard data from other logical streams before handing the TVB to the generic follow machinery.

The MR also adds dedicated HTTP/2 and QUIC multistream capture fixtures plus automated tests selecting a specific stream.

**Architecture rule:** capture-record identity is not sufficient when a multiplexed protocol can carry several logical streams in one packet. Carry the logical stream/substream identifier through the tap contract and filter where the protocol still has authoritative stream identity.

**Testing rule:** include a capture in which at least two logical streams share a packet and verify that following one stream returns only that stream's bytes. Test both CLI and shared follow infrastructure behavior when practical.

**Confidence:** Very high. Merged master implementation with focused regression captures/tests and explicit rationale in the MR description.
