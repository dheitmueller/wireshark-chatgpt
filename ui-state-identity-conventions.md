# UI State Identity Conventions

## Pass stable identity across components and recompute mutable observations locally

Merged master MR !2801 refactors RTP Player live-capture integration so cross-dialog signals and APIs pass `rtpstream_id_t` instead of the larger mutable `rtpstream_info_t` snapshot. RTP Player computes current stream statistics itself.

Rule: when a consumer can derive current mutable state from an authoritative source, pass the smallest stable identity needed to locate that state instead of transporting a snapshot that can become stale.

Confidence: high. Merged master refactor whose stated purpose includes live-capture freshness and simplifying cross-component APIs to stream identity.
