# Wireshark Qt Widget-State Conventions

This file records durable conventions for widget defaults, refreshes, and semantic styling. Current upstream Qt code remains authoritative.

## Apply derived defaults once; do not overwrite later user choices during refresh

Merged master MR !9458, authored and merged by John Thacker, fixes Expert Information so "Limit to display filter" is initialized from the presence of a display filter when the dialog opens, but ordinary `updateWidgets()` calls no longer reset the checkbox. Release-4.0 backport !9460 carries the same fix. The refresh path still updates whether the control is enabled while preserving the user's chosen checked state.

**Implementation rule:** separate one-time default initialization from recurring availability or enablement refresh. A generic widget refresh should not silently restore a derived default over an explicit user choice unless that reset is itself part of the product contract.

## Prefer widget semantic APIs over unsupported embedded markup

Merged master MRs !9455 and !9459, with release backports !9456, !9457, and !9461, fix callers that wrapped ElidedLabel text in HTML-like styling. ElidedLabel escapes rich text after the relevant widget change, so callers now use the widget's `setSmallText()` styling API and pass plain text.

**Implementation rule:** use a custom widget's supported semantic styling or property API rather than injecting markup that the widget treats as text. Escaping behavior is part of the widget contract.

**Confidence:** Very high. Merged master fixes authored by John Thacker with stable-branch backports.

## Match a control's affordance to the item's semantic role

Merged master MR !9091 restores permanently non-checkable extcap multicheck rows to label-like presentation. Tomasz Moń explained that those rows are descriptive device information, not capture options. Guy Harris explicitly agreed that the rows are information about the interface rather than options. If an item can never be acted on, do not make it look like a disabled actionable control merely because it shares a tree with selectable items. Guy separately noted that discoverability may still justify reconsidering where the information belongs in the UI.
