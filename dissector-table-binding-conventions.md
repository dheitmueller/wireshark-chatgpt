# Wireshark Dissector-Table Binding Conventions

This file records durable conventions for dissector-table registration, defaults, and user overrides extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Reset mutable bindings without destroying immutable/default registrations

A dissector-table entry can carry two distinct facts: the original/default registration and the currently selected binding. Preference changes and Decode As operations should mutate the current binding without erasing the original registration that defines the default and remains useful for reset and UI provenance.

Merged master MR !14704, authored by John Thacker and approved/merged by Anders Broman, fixes auto-port preferences associated with Decode As. Removing an old preference value previously called `dissector_delete_uint()`, deleting the table entry itself and thereby losing its initial/default handle. The accepted implementation changes the current handle to `NULL` instead. `dissector_change_uint()` and `dissector_change_string()` delete an entry only when the requested current handle is `NULL` **and** the entry has no initial handle; otherwise they preserve the entry and its initial binding. This also keeps the original dissector visible to Decode As.

**Architecture rule:** when registry state contains both an initial/default value and a mutable current value, treat those as separate layers. Resetting user/configuration state should clear or restore the mutable layer, not delete the structural/default registration. Remove the entry only when there is no underlying default registration to preserve.

**Review rule:** for registration tables that support preferences, Decode As, profiles, or other overrides, verify reset/unset paths as carefully as set paths. A delete operation is stronger than an override reset and can silently destroy information needed to recover the default behavior or present it to the user.

**Confidence:** Very high. Merged master state-management fix authored by John Thacker and approved/merged by Anders Broman, with the default-binding loss and accepted two-layer semantics stated directly in the MR.
