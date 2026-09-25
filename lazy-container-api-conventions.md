# Lazy Container API Conventions

Merged !10362, authored and merged by John Thacker, makes integer wmem-tree lookup and membership operations return the neutral result when the tree pointer is NULL, matching existing string lookup behavior. This lets callers allocate the tree only when inserting the first item.

**API rule:** when a container API intentionally supports lazy creation, read-only lookup and membership operations should treat an absent container consistently as empty when that interpretation is unambiguous. Mutation still establishes the container.

**Confidence:** Very high. Merged core utility change authored and merged by John Thacker, explicitly motivated by lazy creation and consistency with the string APIs.
