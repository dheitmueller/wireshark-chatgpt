# Initialization and lifecycle conventions

## Match resource lifetime to the state it actually depends on

Wireshark's lifecycle hooks are not interchangeable. `init` and `cleanup` routines participate in capture-file/preference lifecycle, while shutdown routines run once when the epan lifetime is ending. Resources whose contents are fixed and independent of the current capture or preferences should therefore be created once at registration/epan lifetime and destroyed at shutdown, rather than rebuilt on every capture or preference change.

**Evidence:** merged master MR !20715 changes the XML encoding `GRegex` from init/cleanup lifetime to registration/shutdown because it is fixed and independent of file/preferences. Merged master MR !20729, authored by John Thacker, applies the same rule to the logcat-text compiled regular expressions. Both changes explicitly distinguish capture/preference lifecycle from process/epan shutdown lifecycle.

**Review rule:** when reviewing an init/cleanup callback, ask what input can actually invalidate the resource. If neither capture state nor preferences affect it, repeated per-capture reconstruction is usually the wrong lifetime.

## Immutable lookup structures should not be rebuilt per dissection or per file

Tables derived entirely from constant generated data belong at a lifetime that matches that constant data. Construct them once in epan scope, and when their size is known, reserve appropriate capacity before bulk insertion. For very large generated constant mappings, also consider whether the generator should emit a sorted table/binary search structure or a perfect hash rather than constructing a dynamic map at runtime.

**Evidence:** John Thacker's merged master MR !20728 moves DICOM constant lookup maps from file scope/repeated initialization to one-time epan-scope creation. Merged follow-up !20730 reserves the exact generated table sizes before inserting thousands of entries and explicitly notes that generated binary/perfect-hash lookup may be a better long-term representation.

## Defer configuration-dependent side effects until effective preferences exist

Startup callbacks can run before profile preferences and command-line overrides have reached their effective values. A callback that is invoked early for structural reasons (for example, UAT loading) must not launch an expensive helper or otherwise commit configuration-dependent behavior based on defaults that may shortly be replaced.

Move the side effect to the preference-application phase or another point where the effective configuration is authoritative. If the desired behavior must occur even when a preference retains its default value, ensure the relevant apply path is still invoked; do not rely only on a detected value change.

**Evidence:** merged master MR !20725, authored by John Thacker, prevents `mmdbresolve` from being started by the first MaxMindDB UAT post-update callback because that callback occurs before all preferences and command-line options are read. The accepted change waits for name-resolution preference application and explicitly marks that module for application so the default-enabled case is handled too.
