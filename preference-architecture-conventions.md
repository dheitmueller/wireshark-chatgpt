# Wireshark Preference Architecture Conventions

This file records durable architectural conventions for preference storage and ownership. Current upstream source remains authoritative.

## Make preference ownership, tree identity, and memory scope explicit rather than singleton-global assumptions

A reusable preference subsystem should not force every caller through one process-global preference tree or one hard-coded allocator scope. The owner of preference state and the lifetime of its storage are part of the subsystem contract; making them explicit allows independent preference trees and standalone preference files to use the same machinery without inheriting unrelated EPAN global state.

Merged master MR !23077, authored and merged by Michael Mann, refactors several preference APIs to remove their singleton globals so callers can create and operate on other preference trees, including UI-layer standalone preference files. Merged !23103 extends the same design by assigning preference modules and preferences an explicit memory scope instead of always using `wmem_epan_scope()` and `g_`-scoped memory. Merged supporting MR !23102 adds `wmem_tree_get_data_scope()` so code can derive the data allocator from the owning tree instead of assuming a global scope.

**Implementation rule:** APIs that manipulate reusable preference state should take or retain the preference-tree/context owner they operate on, and allocation should follow that owner's explicit scope. Do not reach for a process-global singleton or `wmem_epan_scope()` merely because the first consumer lived in EPAN. When a container already owns the lifetime contract, derive or propagate its allocator rather than duplicating that assumption at each caller.

**Review rule:** when extracting preference functionality for another frontend or standalone file, audit hidden globals and allocator assumptions together. Removing a singleton API while leaving storage hard-wired to a global scope only partially decouples the subsystem.

**Confidence:** Very high. Three adjacent merged master changes by Michael Mann form a coherent accepted refactor: generic preference-tree identity, explicit preference memory scope, and access to the owning tree's data scope.

## Use the most semantic preference registration API available

A preference whose value names another Wireshark framework object should be registered with the framework API for that semantic type rather than modeled as an arbitrary string when a typed registration helper exists. Besides documenting intent, the specialized API can apply the validation, lookup, UI behavior, and future framework semantics associated with that object class.

Merged master MR !14555, authored and merged by John Thacker, changes SCCP's payload-dissector preference from `prefs_register_string_preference()` to `prefs_register_dissector_preference()`. The preference is semantically a dissector name, not free-form text; the accepted change also updates its description from "protocol" to "dissector" so the user-facing terminology matches the object being selected.

**Implementation rule:** if a preference selects a dissector, enum, range, file, or another framework-recognized semantic object, prefer the corresponding specialized preference registration API over a generic string/integer representation. Keep the preference's label and help text aligned with the same semantic type.

**Review rule:** when a generic preference contains an identifier later interpreted by a framework registry, check whether a typed preference API already centralizes that validation and lookup. Avoid reimplementing object-name validation at each consumer.

**Confidence:** Very high. Merged master cleanup authored and merged by John Thacker, replacing an existing generic registration with the dedicated dissector-preference API.