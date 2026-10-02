# Resource Handling Conventions

Merged MRs !1731 and !1732, authored by Guy Harris, show that a duplicated file descriptor must be checked before use. If creating the stream wrapper fails, the duplicated descriptor is still owned by the caller and must be closed there.

**Rule:** keep a fallible acquisition result in a named variable until the next ownership-transfer operation succeeds. If that operation fails before taking ownership, release the intermediate resource explicitly.

Merged !1740 also distinguishes textual input validation from the semantic size domain: negative parsed lengths are rejected before conversion, while packet-size limits and validated sizes use unsigned types.

**Rule:** validate malformed negative text before converting it into a nonnegative size representation.

**Confidence:** Extremely high. Merged Guy Harris-authored changes.
