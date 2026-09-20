# Wireshark C API Call-Contract Conventions

This file records durable conventions for low-level C API contracts where pointer validity, return values, error reporting, and ownership transfer affect correctness. Current upstream source and API documentation remain authoritative.

## A zero byte count does not make an invalid pointer argument valid

Do not rely on a zero length to excuse an otherwise invalid pointer passed to a C library or project API. If an API's contract requires a non-NULL pointer, guard the call on pointer validity even when the requested operation can be zero bytes. Compiler attributes and sanitizers may enforce the pointer contract independently of the length value, and the C-level contract should be respected directly rather than depending on a particular libc implementation tolerating the call.

Merged release-4.6 MR !21282, authored and merged by Guy Harris, fixes the C12.22 dissector after UBSan reported a `memcpy()` whose source could be NULL when the configured base OID was absent or malformed. The corresponding length was zero, but the system `memcpy` declaration marked its pointer arguments non-null. The accepted code simply skips the copy when `c1222_baseoid` is NULL.

**Implementation rule:** validate every API precondition independently. In particular, a zero count can mean "there is no work to perform" but does not manufacture a valid object or pointer for a parameter whose contract requires one.

**Confidence:** Extremely high. Merged stable-branch correctness fix authored and merged by Guy Harris, with the sanitizer failure and contract rationale stated explicitly.

## Conditional ownership transfer makes the return status part of the ownership protocol

When a function takes ownership of an argument only if the operation succeeds, callers must inspect the result before deciding whether they still own and must release that object. Ignoring the return value makes ownership unknowable on failure and should be prevented mechanically when practical with a must-use-result annotation.

Merged release-4.6 MR !21244 fixes every affected call to `wtap_block_add_string_option_owned()`. The API transfers ownership of the supplied string when the option is successfully added, but a failure leaves the caller responsible for freeing it. The old callers ignored the return value and therefore leaked strings on failure. The change checks `WTAP_OPTTYPE_SUCCESS`, frees the string on failure, and explicitly argues that this class of ownership-bearing result should be marked `G_GNUC_WARN_UNUSED_RESULT`/`WS_WARN_UNUSED`.

**Implementation rule:** treat success/failure status as part of an ownership-transfer API's type-level contract. On success, follow the documented transfer; on failure, retain or release ownership exactly as documented. For APIs where ignoring the result can create an ownership ambiguity, annotate the result so compilers and checkers force callers to make the decision explicitly.

**Review rule:** whenever an API name includes concepts such as `owned`, `take`, `adopt`, or `steal`, verify whether transfer is unconditional or success-dependent and audit all failure paths accordingly.

**Confidence:** Very high. Merged Wiretap memory-correctness fix with an explicit ownership rationale and accepted call-site corrections.

## Treat the primary status as authoritative; diagnostics may be absent

Do not infer success or failure solely from whether an auxiliary error object was populated. APIs that return a status separately from optional diagnostic detail must be checked in that order: determine success/failure from the documented return status, then consume the diagnostic object if one exists and provide a sensible fallback if it does not.

Merged MR !20953, authored by John Thacker and merged by Michael Mann after Coverity findings, fixes `g_io_channel_read_chars()` handling in the capture synchronization path. The previous code treated a non-NULL `GError` as the indication of failure, but GLib can return `G_IO_STATUS_ERROR` without constructing a `GError`. The accepted code tests `G_IO_STATUS_ERROR` first and reports an unknown error when the diagnostic object is absent.

**Implementation rule:** when an API has both a status/return value and an optional error-detail output, use the status as the control-flow contract. Never dereference or require the detail object merely because the operation failed unless the API explicitly guarantees it.

**Confidence:** Very high. Merged master correctness fix by John Thacker, motivated by concrete static-analysis findings and accepted by Michael Mann.

## Preserve the caller-visible error domain instead of collapsing failures to a generic sentinel

If callers interpret a return value as a particular error domain, return values from every failure path must obey that domain. A generic `-1` is not interchangeable with `-errno` merely because one common failure happens to use errno value 1. Capture volatile error state such as `errno` at the failing operation before cleanup or later calls can overwrite it.

Merged MR !20939, authored by John Thacker and merged by Anders Broman, regularizes `ws80211_utils` so its API stops mixing `-1`, raw errno values, libnl errors, project-specific errors, and direct writes to stderr. It introduces consistent return codes and an explicit error-message retrieval path. Merged follow-up !20961, also authored by John Thacker, fixes `ws80211_iface_up()` to return the actual `-errno` from `socket()`/`ioctl()` failures and saves that value before closing the socket.

**Implementation rule:** define one error-code contract per API boundary and normalize implementation-specific failures into it deliberately. If the contract preserves `errno`, snapshot it immediately at the failure site. Keep human-readable diagnostics in an explicit reporting channel rather than printing unexpectedly from a utility routine.

**Confidence:** Very high. Two merged master changes by John Thacker, including an explicit API-wide cleanup and a concrete follow-up fixing accidental `EPERM`-like behavior.

## Resolve signed error sentinels before converting successful counts to unsigned sizes

For APIs that return a signed count with a negative error sentinel, test the sentinel before any cast to an unsigned size type. If the API accepts an unsigned request length, constrain that input so every valid successful count is representable in the signed return type; this keeps the success and error domains unambiguous.

Merged MR !20978, authored by Guy Harris and merged by John Thacker, tightens the sync-pipe read path so `pipe_read_bytes()` is never asked for a byte count exceeding `SSIZE_MAX`. Callers then check `-1` first and only afterward cast the nonnegative result to `size_t` for comparison with the requested count. Companion !20980 supplies a portable `SSIZE_MAX` definition on Windows so the bound is available there as well.

**Implementation rule:** design count-returning wrappers so valid requests cannot overflow the signed result domain, branch on negative error values first, and cast to unsigned only after nonnegativity is established.

**Confidence:** Extremely high. Merged master change authored by Guy Harris with the signed/unsigned contract spelled out in the commit rationale, plus a merged portability follow-up by John Thacker.

## A wrapper must surface failures that callers need to react to

Checking a lower-level API result locally is not sufficient if the enclosing helper has no way to tell its caller that the operation failed. When successful completion is required for later parsing, authentication, decryption, or state construction, propagate a success/failure result through the wrapper chain until the layer that can make the appropriate policy decision.

Merged master MR !15261 adds error handling for `gcry_md_open()` across multiple dissector/security paths. The library returns zero on success and a nonzero error on failure. Guy Harris specifically reviewed a helper that could notice the failure but could not report it upward and asked for the routine to expose a success/failure indication so the caller could react. The final accepted change checks the library return and propagates failure through the affected wrappers rather than continuing as if a digest context had been created. Guy also used the same review to reinforce Wireshark's migration from `gboolean` to C99 `bool` for project-owned interfaces.

**Implementation rule:** if a nested API failure invalidates the promised result of the current helper, make that failure part of the helper's contract and propagate it. Do not convert "initialization failed" into a partially initialized success merely because the original wrapper did not previously have an error return.

**Review rule:** when adding a return-value check inside a `void` or unconditional-success helper, follow the call graph upward. Ask which caller must change behavior after failure and extend the status contract far enough to reach that decision point.

**Confidence:** Extremely high. Merged master correctness change with explicit Guy Harris review identifying the missing propagation boundary, followed by an accepted implementation that carries the failure upward.

## Check nullable pointer results before pointer arithmetic, not afterward

A pointer-returning search or lookup API must be tested for its failure sentinel before the pointer is adjusted, dereferenced, or otherwise transformed. Adding an offset to NULL is already invalid C behavior; checking the adjusted pointer afterward does not recover the original failure and can hide a defect behind platform-specific behavior.

Merged master MR !15167, authored and merged by John Thacker after a Coverity finding in the HTTP dissector, moves the NULL check on `ws_strnstr()` ahead of the `+ 3` adjustment used to skip a delimiter. The old code formed `NULL + 3` on a failed search and only then tested the resulting pointer.

**Implementation rule:** preserve the API's original result long enough to evaluate its success/failure contract. For nullable pointer returns, branch on NULL before any arithmetic, field access, cast-dependent dereference, or ownership transfer; perform offsets only inside the known-success path.

**Review rule:** expressions such as `search(...) + n`, `lookup(...)->field`, or a pointer-returning call embedded inside another expression deserve scrutiny whenever the callee can return NULL. Refactor to an explicit temporary and check the sentinel first.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with a concrete static-analysis report exposing undefined pointer arithmetic.
