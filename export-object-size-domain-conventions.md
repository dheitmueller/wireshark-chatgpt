# Export Object Size Conventions

MR 3271 changes the Export Object payload length to size_t. This follows Guy Harris review in MR 3268: an object stored in process memory is bounded by the addressable size domain, even when a protocol can describe a wider length.

Rule: use size_t for the size of a resident object or buffer. Keep wider protocol lengths separate, and check that they fit before converting them to a resident-object size.

Confidence: extremely high. Direct Guy Harris review was followed by the merged implementation.
