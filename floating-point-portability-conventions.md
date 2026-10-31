# Wireshark Floating-Point Portability Conventions

This file records durable conventions for floating-point behavior where compiler and architecture choices can affect observable Wireshark output or regression tests. Current upstream source and build configuration remain authoritative.

## Make required precision semantics explicit when deterministic output matters

Do not assume that an assignment or cast necessarily forces every supported compiler/architecture combination to round an intermediate floating-point result exactly as expected by a regression test. Toolchains that retain excess intermediate precision can produce different results at boundary values even when the source-level algorithm is otherwise identical.

Merged master MR !21411, authored and merged by John Thacker, fixed `format_units` test instability on x86 targets using the x87 FPU. GCC in GNU C dialects could retain 80-bit intermediate precision rather than rounding intermediates to the precision implied by the C abstract machine at the points expected by the code. The difference only affected marginal boundary values, but it made output and tests architecture/toolchain dependent. The accepted fix applies `-fexcess-precision=standard` specifically to `wsutil/str_util.c`; release backport !21414 carries the same correction.

**Implementation rule:** when Wireshark requires deterministic floating-point formatting or boundary behavior across supported targets, identify whether the compiler is permitted to retain excess precision and explicitly select the required language semantics when necessary. Prefer a narrow source/target-scoped compiler option over a project-wide flag when only one component needs the constraint. Do not paper over a known semantic difference by loosening exact regression expectations if the intended behavior can instead be made deterministic.

**Confidence:** Very high. Merged master portability/test correction authored and merged by John Thacker, with a stable-branch backport.

## Test floating-point classification results for truth, not an exact integer

In merged MinGW cleanup !9436, Guy Harris explained that C11 guarantees `isinf(x)` returns a **nonzero** value for an infinity, not `1` or `-1` specifically. Some implementations distinguish positive from negative infinity, and code that tests `isinf(x) == 1 || isinf(x) == -1` is not portable even if those values are commonly observed. The accepted Synphasor dissector change simply tests `if (isinf(pmu_lat))` and equivalent longitude/elevation cases.

**Rule:** treat `isinf`, `isnan`, and analogous classification predicates according to their standards-defined Boolean contracts. Avoid depending on specific nonzero return values or signs. **Confidence:** exceptionally high, explicit Guy Harris standards review and merged correction.
