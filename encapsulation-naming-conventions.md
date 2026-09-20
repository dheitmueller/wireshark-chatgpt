# Wireshark Encapsulation Naming Conventions

This file records durable conventions for naming capture encapsulations and related wire/file-format abstractions. Current upstream source remains authoritative.

## Name an encapsulation for what it guarantees, not its original use case

A link type or encapsulation name should describe the semantics actually present in every record of that format. Historical motivation is not a sufficient naming contract when the same framing is valid for other higher-level protocols.

Merged master MR !15430 was authored and merged by Guy Harris. Linktype 209 began as `LINKTYPE_IPMB_LINUX` for IPMB-over-I2C, but its pseudo-header contains only I2C information and the same link type is also used for HDMI DDC-over-I2C. Because captures using it are not guaranteed to contain IPMB, the upstream linktype had been renamed `LINKTYPE_I2C_LINUX`; Guy's MR updates Wireshark's displayed encapsulation name and comments to match the actual I2C-level guarantee.

**Implementation rule:** name capture/link-layer abstractions at the lowest semantic layer the format actually guarantees. Do not retain a protocol- or application-specific label merely because that was the format's first use if valid records can carry other traffic on the same substrate.

**Confidence:** Extremely high. Merged master naming correction authored and merged by Guy Harris, with the semantic rationale stated directly in the commit.
