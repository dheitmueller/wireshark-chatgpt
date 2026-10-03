# Qt Palette-State Conventions

This file records durable guidance for Qt UI state derived from the active application palette or theme. Current upstream Qt code remains authoritative.

## Recompute theme-derived values when the application palette changes

A value derived from `QApplication::palette()` at widget construction time is not stable for the lifetime of the widget. Desktop/theme changes can replace the application palette while the process is running.

Merged master MR !508, authored by Gerald Combs, changes syntax-filter highlighting to choose a contrasting foreground from the current application palette and the configured state background. It removes a foreground value cached at construction and explicitly handles `ApplicationPaletteChange`, so existing widgets update when dark/light palette state changes.

**UI rule:** when a widget caches colors, brushes, style sheets, or other presentation state derived from the current palette, either recompute that state on palette/theme change or keep it expressed through live palette roles. Do not assume construction-time palette values remain valid for the widget's entire lifetime.

**Confidence:** Very high. Merged master Qt change authored by Gerald Combs with the palette-change lifecycle implemented directly.
## Rebuild palette-dependent scene objects, not just top-level palette state

Handling `ApplicationPaletteChange` at the application level is not enough when a complex widget has already materialized colors into scene items. The widget must invalidate or rebuild those cached graphical objects so the new palette reaches what is actually rendered.

Merged master MR !436, authored by Gerald Combs, gives the packet diagram an explicit `resetScene()` path and handles `ApplicationPaletteChange` by rebuilding the scene. This complements !508's application-palette rule: palette-derived values must propagate through every cache layer that stores them.

**UI rule:** trace theme-derived state all the way to rendered objects. If a scene/model caches brushes, pens, text colors, or geometry derived from the old palette, invalidate or recreate that layer on palette change instead of updating only a global preference flag.

**Confidence:** Very high. Merged master Qt change authored by Gerald Combs.

## Match reset scope to state lifetime

`QGraphicsScene::clear()` deletes items but intentionally preserves other scene state such as the scene rectangle. That behavior can be useful when switching packets because it retains scroll position, but it is wrong when switching captures and stale scene extent should be discarded.

Merged master MR !417 distinguishes per-packet clearing from a hard reset when the capture file changes.

**Lifecycle rule:** choose reset operations according to the lifetime of the state they invalidate. Preserve useful within-capture UI state across packet changes, but fully reconstruct state whose validity ends with the capture.

**Confidence:** Very high. Merged master Qt lifecycle fix authored by Gerald Combs.
