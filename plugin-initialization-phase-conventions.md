# Plugin Initialization Phase Conventions

Merged MR !7060, authored by João Valverde, adds an EPAN plugin `post_init()` callback because the existing plugin `init()` runs before `proto_init()`, while some plugin work requires fully initialized EPAN/protocol state.

**Rule:** when extension callbacks have different prerequisites, represent those prerequisites as explicit lifecycle phases. Avoid relying on incidental startup ordering or forcing late-stage work into an early callback.

**Confidence:** High. Merged core plugin-lifecycle change.
