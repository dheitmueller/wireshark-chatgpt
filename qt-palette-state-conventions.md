# Qt Palette-State Conventions

This file records durable guidance for Qt UI state derived from the active application palette or theme. Current upstream Qt code remains authoritative.

## Recompute theme-derived values when the application palette changes

A value derived from `QApplication::palette()` at widget construction time is not stable for the lifetime of the widget. Desktop/theme changes can replace the application palette while the process is running.

Merged master MR !508, authored by Gerald Combs, changes syntax-filter highlighting to choose a contrasting foreground from the current application palette and the configured state background. It removes a foreground value cached at construction and explicitly handles `ApplicationPaletteChange`, so existing widgets update when dark/light palette state changes.

**UI rule:** when a widget caches colors, brushes, style sheets, or other presentation state derived from the current palette, either recompute that state on palette/theme change or keep it expressed through live palette roles. Do not assume construction-time palette values remain valid for the widget's entire lifetime.

**Confidence:** Very high. Merged master Qt change authored by Gerald Combs with the palette-change lifecycle implemented directly.
