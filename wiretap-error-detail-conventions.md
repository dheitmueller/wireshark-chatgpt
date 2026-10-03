# Wiretap Error-Detail Conventions

## Pair stable error categories with owned diagnostic detail

A numeric Wiretap error is a machine-readable category, but a lower layer can often provide important context that the category cannot encode. Preserve both.

Merged master MR !579, authored and merged by Guy Harris, extends Wiretap dump open, finish, and close contracts with an `err_info` out-parameter. APIs initialize it to NULL, format-specific paths populate it for errors such as impossible pcapng interface state, and callers carry the detail to the reporting layer. Guy's merged follow-up !580 improves the user-facing write-error message to include the added context.

**API rule:** keep the stable numeric code for programmatic decisions and carry an optional detail string for the concrete invariant or failure. Initialize both outputs deterministically.

**Ownership rule:** if the detail is allocated, make ownership explicit and free it on every consuming path. Richer diagnostics add lifetime obligations.

**Propagation rule:** do not replace a detailed lower-layer failure with a generic message at an intermediate helper boundary. Preserve the code and detail until the final UI/CLI reporter.

**Confidence:** Extremely high. Broad Wiretap API change and follow-up authored and merged by Guy Harris.
