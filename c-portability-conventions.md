# C Portability Conventions

This file records durable C-language portability conventions extracted from accepted Wireshark changes. Current upstream coding and compiler requirements remain authoritative.

## Use C17-compatible aggregate zero initialization across supported compilers

Wireshark code targeting the project's C17 baseline should use `{0}` when an aggregate needs explicit zero initialization rather than relying on the empty initializer `{}`, which was standardized only in C23 and is not accepted by all supported older compiler versions.

Merged MR !22519 fixes NATS compilation with Visual Studio 2022 17.8 by replacing `tokens[...] = {}` with `tokens[...] = {0}`. John Thacker explicitly requested `{0}`, noting that it works across MSVC versions while the empty form only entered the C standard in C23. He also pointed out that preserving the existing zero-initialization semantics makes the change much easier to review than removing initialization and requiring reviewers to prove it unnecessary.

**Implementation rule:** when equivalent syntax differs in language-version or compiler support, use the form valid under Wireshark's declared language baseline and supported toolchains. For portability fixes, prefer the smallest semantic-preserving change; do not broaden a compiler-compatibility patch into removal of behavior unless that behavior has independently been shown unnecessary.

**Confidence:** Very high. Merged master compiler-portability fix with explicit John Thacker review and approval.

## Make statement-like macros use ordinary statement syntax at the call site

A function-like macro intended to behave as a statement should not hide a trailing semicolon in the macro replacement text. Call it with the ordinary `MACRO();` syntax instead. Keeping the semicolon at the invocation makes the construct read and parse like a normal statement and avoids extra-semicolon diagnostics when stricter compiler warnings are enabled.

Merged MR !21037 adds C and C++ testing for `-Wextra-semi`. During review, John Thacker identified existing macros with embedded trailing semicolons as one of the blockers. Michael Mann stated his preferred style explicitly: the semicolon should follow the macro invocation and not be included in the macro, i.e. `MY_MACRO();`; John agreed that this was also his preference. The prerequisite cleanup was tracked through merged !21018.

**Implementation rule:** define statement-like macros without a trailing semicolon and write the semicolon at each invocation, just as for a function call. When enabling stricter warnings, fix the macro definition/call contract rather than sprinkling exceptional syntax around callers.

**Confidence:** Very high. Explicit, agreeing review guidance from Michael Mann and John Thacker, followed by a merged compiler-warning change after the cleanup.

## Do not spell the minimum signed value with an out-of-range positive literal

In C, a leading minus sign is an operator, not part of an integer constant. A spelling such as `-2147483648` is therefore formed by first choosing a type for the positive constant `2147483648` and then applying unary minus. On a compiler/data model where that positive value is not representable as `int`, the literal can acquire an unsigned or wider type and produce warnings or semantics different from the intended “smallest int” value.

Merged master MR !12079 was authored by Guy Harris after a Visual Studio warning and replaces an open-coded `-2147483648` sentinel with `INT_MIN`. Guy's rationale is explicit: the program wants the implementation's minimum `int`, so the standard header/compiler should provide the representation rather than source code trying to manufacture it from a positive literal whose type selection is implementation-sensitive. He notes that Visual Studio versions may define `INT_MIN` as an expression such as `(-2147483647 - 1)` precisely to avoid the problematic literal.

**Implementation rule:** use the standard limits macros (`INT_MIN`, `INT32_MIN`, and the corresponding limits for the actual type) when code means the minimum representable signed value. Do not hard-code the value by applying unary minus to a decimal literal that may already be outside the positive range of the intended signed type.

**Review rule:** when a compiler warns about a boundary integer constant, inspect the type of the literal before the unary operator as well as the destination type. Source text that looks numerically correct can still have different C type semantics across supported compilers.

**Confidence:** Extremely high. Merged master portability fix with the language-semantics rationale authored by Guy Harris.

## Do not treat an operating-system family as one uniform header/API surface

Platform-specific includes should be driven by an actual dependency, not by a broad family label. Closely related operating systems can expose different private or implementation headers; an unconditional include that happens to work on one BSD can therefore break another BSD even when the code itself is otherwise portable.

Merged master MR !13777 was authored and merged by Guy Harris and removes an unnecessary `net/if_var.h` include from the interface-monitor code. Guy states the portability policy explicitly in the MR description: the header is not present on all BSDs (NetBSD does not have it), and if a future build shows that some BSD genuinely requires it, the include should be restored only for the BSD or BSDs that need it rather than unconditionally for the whole family.

**Implementation rule:** remove platform-private includes that are not actually needed. If a dependency is proven necessary on only part of an OS family, guard it narrowly for the platforms/versions where the requirement exists; do not generalize from one member of the family to all of them.

**Review rule:** for portability failures, distinguish “this platform family uses similar APIs” from “every supported member provides this exact header or declaration.” Prefer evidence from the failing toolchain/platform and the smallest conditional compatibility scope that fixes it.

**Confidence:** Extremely high. The merged master change and the narrow-scoping guidance were both authored by Guy Harris.

## Match generic-library callbacks to the exact element type they receive

Callbacks passed to generic C library routines such as `bsearch()` are invoked on objects with the exact layout and alignment of the array being searched. A comparator written for a different structure type is not made safe merely because the two structures happen to begin with similar-looking members. Reusing it through casts can impose the wrong alignment or member-offset assumptions and becomes undefined or fault-prone on stricter architectures.

Merged master MR !11809, authored and merged by João Valverde, fixes an unaligned-memory access in the manuf lookup code by giving `bsearch()` a comparator that matches the actual array element type instead of reusing a comparator for a different structure layout. The bug was architecture-sensitive, which is exactly the class of failure hidden by relying on coincidental layout compatibility.

**Implementation rule:** write a comparator/callback against the real object type supplied by the library API, or explicitly factor a common key representation that both callers can safely access. Do not cast between unrelated structure types to share callback code based on assumed prefix layout or alignment.

**Review rule:** when a callback receives `void *`, trace the concrete type the calling library will pass at runtime. Treat casts that reinterpret it as a different struct as portability red flags even when they appear to work on x86.

**Confidence:** Very high. Merged master portability/correctness fix authored and merged by João Valverde with a concrete unaligned-access failure mode.
