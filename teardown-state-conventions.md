# Teardown State Conventions

Merged MR 5053 documents that Qt widgets assume epan data remains valid, so the main window must be destroyed before epan_cleanup().

Merged MR 5028 makes Lua and funnel dialogs children of the main window so normal Qt parent ownership gives them predictable destruction order.

Merged MR 5011, with backport 5012, clears tap_listener_queue and tap_dissector_list after cleanup so later teardown code does not observe stale global state.

Rules:
- Destroy consumers before subsystem state they depend on.
- After cleanup, restore globally reachable state to its neutral empty value when later code can still observe it.
- Prefer the object ownership model to encode destruction order.

MRs 5027 and 5029 revert an earlier Qt cleanup fix, so that reverted implementation is negative history rather than current guidance.

Confidence: very high. Merged master changes plus stable corroboration.
