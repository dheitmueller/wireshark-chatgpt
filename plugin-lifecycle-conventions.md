# Wireshark Plugin Lifecycle Conventions

This file records durable lifecycle rules for dynamically loaded code and callbacks registered with longer-lived Wireshark subsystems. Current upstream source remains authoritative.

## Do not unload dynamic code while registered callbacks can still execute

The lifetime of a plugin is constrained by every callback, destructor, or function pointer that another live subsystem retains. Unloading the module before those callback sources have completed teardown can turn otherwise routine allocator cleanup into a call through code that no longer exists.

Merged master MR !13978 fixes the epan shutdown ordering directly: plugins can register callbacks that execute while wmem allocators are emptied or destroyed, so `plugins_cleanup()` must run only after `wmem_cleanup_scopes()` has invoked those callbacks. The accepted diff moves plugin unloading below wmem-scope cleanup; John Thacker approved and merged it. Merged stable backports !13982 (release-4.2), !13996 (release-4.0), and the later release-3.6 MR !15684 carry the same ordering rule across maintained branches.

**Implementation rule:** before unloading a plugin or other dynamic module, destroy or unregister every framework object capable of invoking code owned by that module. Treat allocator destroy callbacks, taps, listeners, timers, event hooks, and equivalent retained function pointers as dependencies in the shutdown ordering graph.

**Review rule:** reason about teardown in reverse dependency order. It is not sufficient that the plugin has finished its ordinary work; all callback-producing owners that can still reach plugin code must be quiesced first.

**Confidence:** Extremely high. Direct merged master crash fix approved/merged by John Thacker, plus accepted stable backports across release-4.2, release-4.0, and release-3.6.