# Wireshark Follow-Stream Extension Conventions

This file records durable architecture and UI review guidance from accepted Follow Stream changes. Current upstream registration and GUI code remain authoritative.

## Generate Follow actions from protocol registration rather than an enum

Merged master MR !9957, authored by John Thacker, replaces the closed `follow_type_t` enumeration with protocol IDs for Follow Stream identity and dialog construction. The frontend builds Follow actions from registered followers; dissectors, including plugins, can therefore register Follow capability without adding a new constant and duplicating menu wiring in several windows. Conversation and Endpoint dialogs no longer need bespoke assumptions that only particular transports can be followed; for example DCCP becomes available through the same mechanism.

**Architecture rule:** extension points should drive their own discoverable capabilities through stable registration and protocol identity. Prefer the registration API and a single dynamic menu/action construction path over a growing parallel UI enum and per-protocol switch cases. Preserve explicit exceptional logic only where the domain requires it (e.g., shortcuts, unusual menu names, and cases where mere protocol presence does not prove the stream is actually followable).

**Testing rule:** verify registration/availability from built-in and plugin dissectors, multiple simultaneous followable protocols, and the corresponding Conversation/Endpoint as well as packet context menus.

**Confidence:** Very high for the registration architecture. A substantial merged master implementation authored by John Thacker.

## Empty dynamic menus are an explicit usability state

In the discussion on !9957, Chuck Craft reported that an enabled Follow submenu with no entries appears to be loading but never becomes available. John Thacker agreed that disabling an empty menu made sense and rejected always listing all unsupported protocol options, which could grow arbitrarily as plugin registrations increase.

**UI review rule:** distinguish a disabled/no-options state from an enabled action that opens an empty dynamic submenu. A menu populated from installed plugins and current-frame capabilities must expose emptiness clearly. Keep unavailable protocol options out of long, misleading lists.

**Evidence qualification:** this is explicit authoritative **post-merge UI feedback**, not proof that every subsequently shipping branch implements the disabled-empty-menu behavior. Check current UI source before treating it as implemented.

**Confidence:** High for the usability direction (direct John Thacker feedback), with implementation status intentionally left unasserted.
