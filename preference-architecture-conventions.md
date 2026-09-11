# Wireshark Preference Architecture Conventions

This file records durable architectural conventions for preference storage and ownership. Current upstream source remains authoritative.

## Make preference ownership, tree identity, and memory scope explicit rather than singleton-global assumptions

A reusable preference subsystem should not force every caller through one process-global preference tree or one hard-coded allocator scope. The owner of preference state and the lifetime of its storage are part of the subsystem contract; making them explicit allows independent preference trees and standalone preference files to use the same machinery without inheriting unrelated EPAN global state.

Merged master MR !23077, authored and merged by Michael Mann, refactors several preference APIs to remove their singleton globals so callers can create and operate on other preference trees, including UI-layer standalone preference files. Merged !23103 extends the same design by assigning preference modules and preferences an explicit memory scope instead of always using `wmem_epan_scope()` and `g_`-scoped memory. Merged supporting MR !23102 adds `wmem_tree_get_data_scope()` so code can derive the data allocator from the owning tree instead of assuming a global scope.

**Implementation rule:** APIs that manipulate reusable preference state should take or retain the preference-tree/context owner they operate on, and allocation should follow that owner's explicit scope. Do not reach for a process-global singleton or `wmem_epan_scope()` merely because the first consumer lived in EPAN. When a container already owns the lifetime contract, derive or propagate its allocator rather than duplicating that assumption at each caller.

**Review rule:** when extracting preference functionality for another frontend or standalone file, audit hidden globals and allocator assumptions together. Removing a singleton API while leaving storage hard-wired to a global scope only partially decouples the subsystem.

**Confidence:** Very high. Three adjacent merged master changes by Michael Mann form a coherent accepted refactor: generic preference-tree identity, explicit preference memory scope, and access to the owning tree's data scope.
