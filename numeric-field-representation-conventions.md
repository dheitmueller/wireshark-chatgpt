# Wireshark Numeric Field Representation Conventions

This file records durable conventions for representing numeric protocol values in the protocol tree. Current upstream source remains authoritative.

## Represent numeric measurements as numeric fields with unit metadata

A protocol value that is semantically numeric should remain numeric in the protocol tree even when a formatted string looks attractive in the GUI. Encoding the value as `FT_STRING` makes ordinary range comparisons and numeric display filters impossible and duplicates presentation logic that Wireshark's field/unit machinery already provides.

In release-branch MR !9882, Gilbert Ramirez reviewed a TECMP voltage field that was rendered as a string and explicitly suggested making it `FT_DOUBLE` so users could filter for values below or above the measurement. Merged master MR !9887 implements that direction: the dissector computes the voltage as a `double`, registers the field as `FT_DOUBLE`, and attaches `units_volt` through `BASE_UNIT_STRING`.

**Implementation rule:** model measurements, counters, rates, and other numeric protocol concepts with the appropriate numeric `FT_*` type and standard unit metadata. Keep human formatting in the display layer; do not turn a number into a string solely to append units or control decimal presentation.

**Review rule:** when a field is registered as text but is parsed arithmetically, ask whether users should be able to perform numeric display-filter comparisons. If yes, the field type is probably carrying presentation instead of semantics.

**Confidence:** Very high. Direct Gilbert Ramirez review followed immediately by a merged master implementation adopting the numeric field representation.
