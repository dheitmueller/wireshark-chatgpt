# Wireshark Capture Option Length-Budget Conventions

This file records durable conventions for validating length-limited capture-file options and their payloads. Current upstream source and format specifications remain authoritative.

## Budget fixed option overhead before validating variable payload length

If an on-disk length field describes the entire option body, the maximum variable payload is smaller than the field's numeric maximum whenever the option body also contains fixed discriminator, type, version, or other overhead bytes. Size-counting and write paths must enforce the same effective bound.

Merged master MR !15322 was authored and merged by Guy Harris and fixes pcapng `IDB_FILTER` sizing. The pcapng option body length is a 16-bit quantity, but the first byte of the filter option is the filter type; therefore the filter string must be shorter than `UINT16_MAX - 1`, not merely shorter than `UINT16_MAX`. The actual writer already used the correct bound, while the code that counted the option length used the looser one; the accepted fix makes the two paths agree. Stable-branch backports !15323, !15324, and !15325 corroborate the correctness importance of the change.

**Implementation rule:** derive the variable-payload limit from the serialized container budget: `encoded maximum - fixed body overhead`, with padding/terminators included according to the format. Do not compare only the user payload against the raw maximum of the enclosing length field.

**Consistency rule:** any preflight/counting routine, size calculator, validation routine, and actual writer must use the same effective bound. A writer that rejects data after a counter has accepted it can produce incorrect allocation/length calculations even if the final serialization check is correct.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by Guy Harris, with three accepted stable-branch backports and an explicit explanation of the one-byte option-body overhead.