# Dependency migration conventions

This file records durable conventions for staged removal of source-level dependency APIs and headers. Current upstream implementation remains authoritative.

## Remove a dependency include only after its use sites have actually migrated

A directory or library is not dependency-free merely because a common header stopped including that dependency. If source files still use dependency-specific macros, types, or helpers, keep the dependency visible at those use sites until those constructs have been replaced with the intended portable equivalents.

Closed MR !12313 exposed this during the effort to reduce GLib dependence in `wsutil`. Jaap Keuter noted that source files still used `G_GUINT64_CONSTANT()` after a shared header stopped bringing in `glib.h`. Guy Harris gave the high-authority portability analysis: the C99/C11 `UINT64_C()` macro is the rough standard equivalent, technically defined for `uint_least64_t`, while Wireshark's supported platforms can reasonably expect the relevant 64-bit types to coincide. Jaap proposed keeping `glib.h` localized in files that still use GLib constructs until a broader conversion is complete. Gerald Combs then moved that conversion into a separate change, and merged master MR !12334 updated the conversion tooling and migrated the applicable `epan`, `ftypes`, and `wsutil` uses to `INT64_C()` / `UINT64_C()`.

**Implementation rule:** dependency removal should proceed from use-site migration to include removal, not the reverse. During a staged conversion, explicitly include the dependency in remaining users so their requirements stay visible; once dependency-specific constructs are gone, remove the now-unneeded include or transitive dependency.

**Portability rule:** when replacing a dependency-specific portability macro with a language-standard equivalent, verify the standard's exact type contract and the assumptions Wireshark makes about its supported platforms. Do not treat similarly named macros as interchangeable without checking the represented type domain.

**Submission rule:** broad mechanical migration is easier to review and reason about when separated from the header/dependency-graph cleanup that motivated it. Keep conversion tooling synchronized with the source transformation so later contributors do not reintroduce the old dependency form.

**Confidence:** High. The motivating MR was closed, so its implementation is not precedent, but Guy Harris's standards analysis is highly authoritative and its proposed direction was realized immediately in merged master MR !12334.