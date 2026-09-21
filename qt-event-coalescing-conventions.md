# Qt event coalescing conventions

## Coalesce high-frequency events before expensive rendering

When a Qt input signal can arrive much faster than an expensive render or replot can complete, do not synchronously redraw for every event. Queue or coalesce the redraw into the event loop so multiple input events collapse into the latest necessary visual update.

Merged master MR !14854, authored and merged by John Thacker, changed I/O Graph mouse-move handling from an immediate `replot()` to `QCustomPlot::rpQueuedReplot`. The MR notes that Linux `mouseMoveEvent` delivery can be as frequent as roughly 25 microseconds and that queuing the replot dramatically improves performance.

The reusable rule is broader than QCustomPlot: keep high-rate event handlers lightweight and schedule expensive, idempotent visual work so redundant intermediate states are not rendered. Use an immediate redraw only when every intermediate state is semantically observable or otherwise required.

**Confidence:** High. Merged master change authored and merged by John Thacker, with a concrete measured event-frequency motivation.

## Throttle expensive latest-state calculations when queuing alone is not enough

Some high-frequency UI work is not just a redraw. Hover hit-testing, nearest-item searches, or other expensive calculations may still run once for every input event even if the eventual paint is coalesced. In that case, rate-limit the calculation itself and retain only the newest input state needed by the next calculation.

Merged master MR !14817, authored by John Thacker and merged by Anders Broman, fixes RTP Player mouse tracking. `QCustomPlot::plottableAt()` ultimately performs costly distance calculations against graph line segments; on Linux, mouse events could arrive roughly every 25 microseconds and make the GUI unresponsive. The accepted implementation stores the latest mouse position, starts a single-shot `QTimer` only when one is not already active, and performs the expensive hover/highlight work when the timer fires. Intermediate positions are deliberately discarded because only the latest position matters to the UI.

**Implementation rule:** distinguish coalescing from throttling. For an idempotent redraw, a queued render may be sufficient. For expensive event-derived computation, keep the event handler to cheap state capture, keep one pending timer/work item, and process the latest state at a bounded cadence. Do not build an unbounded queue of stale mouse positions or perform expensive hit-testing for positions the user has already moved past.

**Confidence:** Very high. Merged master performance fix authored by John Thacker, with a documented platform-specific event rate and a concrete latest-state timer design.
