# Qt event coalescing conventions

## Coalesce high-frequency events before expensive rendering

When a Qt input signal can arrive much faster than an expensive render or replot can complete, do not synchronously redraw for every event. Queue or coalesce the redraw into the event loop so multiple input events collapse into the latest necessary visual update.

Merged master MR !14854, authored and merged by John Thacker, changed I/O Graph mouse-move handling from an immediate `replot()` to `QCustomPlot::rpQueuedReplot`. The MR notes that Linux `mouseMoveEvent` delivery can be as frequent as roughly 25 microseconds and that queuing the replot dramatically improves performance.

The reusable rule is broader than QCustomPlot: keep high-rate event handlers lightweight and schedule expensive, idempotent visual work so redundant intermediate states are not rendered. Use an immediate redraw only when every intermediate state is semantically observable or otherwise required.

**Confidence:** High. Merged master change authored and merged by John Thacker, with a concrete measured event-frequency motivation.
