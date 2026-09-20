# Wireshark Plugin Lifecycle Conventions

This file records durable lifecycle rules for dynamically loaded code and callbacks registered with longer-lived Wireshark subsystems. Current upstream source remains authoritative.

## Do not unload dynamic code while registered callbacks can still execute

The lifetime of a plugin is constrained by every callback, destructor, or function pointer that another live subsystem retains. Unloading the module before those callback sources have completed teardown can turn otherwise routine allocator cleanup into a call through code that no longer exists.

Merged release-3.6 MR !15684 backports the fix for an epan shutdown crash. Plugins can register callbacks that run when wmem allocators are emptied or destroyed, so `plugins_cleanup()` must occur only after `wmem_cleanup_scopes()` has invoked those callbacks. The accepted diff moves plugin unloading after wmem-scope cleanup; John Thacker approved and merged the backport. The MR identifies the corresponding merged master commit, so the stable-branch change corroborates an already accepted master lifecycle ordering rather than introducing release-only behavior.

**Implementation rule:** before unloading a plugin or other dynamic module, destroy or unregister every framework object capable of invoking code owned by that module. Treat allocator destroy callbacks, taps, listeners, timers, event hooks, and equivalent retained function pointers as dependencies in the shutdown ordering graph.

**Review rule:** reason about teardown in reverse dependency order. It is not sufficient that the plugin has finished its ordinary work; all callback-producing owners that can still reach plugin code must be quiesced first.

**Confidence:** Very high. Stable-branch backport of a merged master crash fix, approved and merged by John Thacker, with the callback-after-unload mechanism and one-line ordering correction explicit in the MR.