# Wireshark C API Call-Contract Conventions

This file records durable conventions for low-level C API contracts where pointer validity, return values, and ownership transfer affect memory correctness. Current upstream source and API documentation remain authoritative.

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