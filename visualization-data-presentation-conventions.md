# Wireshark Visualization Data Presentation Conventions

This file records durable conventions for separating visualization data from presentation formatting. Current upstream source remains authoritative.

## Keep collected graph data in canonical units and format units at the presentation boundary

Human-readable scaling should normally be a presentation concern. Avoid rewriting an entire collected data set merely to change axis labels, SI prefixes, or other display formatting when the underlying measurement has not changed.

Merged master MR !15104, authored by John Thacker, replaces I/O Graph's data-rescaling pass with a `QCPAxisTickerSi` that formats tick labels using a reusable `format_units()` helper. The graph retains its original values while the axis chooses SI prefixes and units at rendering time. Besides simplifying unit handling, this removes many floating-point operations over busy graph data and makes smaller graph intervals more practical.

**Architecture rule:** keep the cached/tapped measurement in one stable semantic unit and carry enough metadata to format it for presentation. Unit-prefix changes, locale formatting, and axis-label choices should not mutate the measurement cache unless the graph's actual semantic unit changes.

**Performance rule:** when a display transformation can be expressed at tick/label rendering time, prefer that over an O(n) rewrite of all points. This also makes mixed-view behavior easier to reason about because presentation state no longer changes the stored data.

**Confidence:** Very high. The evidence is a merged master change authored by John Thacker and accepted by Anders Broman that deliberately replaces data mutation with presentation-time formatting for performance and maintainability.
