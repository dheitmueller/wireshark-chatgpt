# Dissector Table Key Ownership Conventions
Merged MR 5997 fixes a leak by extending custom dissector-table registration with a key-destroy callback. Flat heap keys use g_free; compound COSE keys release referenced GVariant members before freeing the outer key.

Rule: an API that lets callers define key representation, hashing, and equality must also make destruction and ownership explicit. The owning container should invoke that destructor on replacement, removal, or destruction. Compound-key cleanup must release owned nested resources before freeing the key.

Confidence: very high; merged core API and caller updates.
