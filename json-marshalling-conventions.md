# Wireshark JSON Marshalling and Dumper State Conventions

This file records accepted historical lessons from sharkd and `wsutil/json_dumper`. Current upstream source remains authoritative.

## Distinguish named object members from nameless array elements

JSON member names are required when writing a value into an object, but array elements are positional and have no member name. A serializer wrapper that always calls `json_dumper_set_member_name()` before emitting a value is wrong for array elements, even if it happens to work for some primitive helpers.

Merged master MR !9988, authored by Guy Harris, changes sharkd base64 and object-opening wrappers to call `json_dumper_set_member_name()` **only when the key is non-null**, permitting those values to be emitted as nameless elements of arrays. Merged master MR !9968, also authored by Guy Harris, introduces paired `sharkd_json_object_open()` and `sharkd_json_object_close()` helpers and applies them across nested response paths.

**Implementation rule:** model the container contract explicitly: name values under objects, omit names inside arrays, and use shared begin/end wrappers so nested JSON is balanced and later changes do not strand the dumper at the wrong depth.

**Confidence:** Extremely high. Two merged mainline sharkd fixes authored by Guy Harris.

## Centralize stack depth and sticky error-state checks in the dumper

The JSON dumper, not each individual caller, owns the nesting-depth invariant. Splitting overflow, underflow and error-flag responsibilities across callers invites inconsistent diagnostic detail and allows later code to use a corrupted stack index after an earlier failure.

Merged master MR !9999, authored by Guy Harris, explains a rework that centralizes stack-depth checks for both push and pop, has `json_dumper_bad()` set `JSON_DUMPER_FLAGS_ERROR`, and reports the current/previous state in errors rather than relying on callers to re-set the flag. Its corpus JSON contains only a short captured diff fragment; these details are taken from the accepted MR description and require checking full source on the target branch before applying exact helper names or assumptions.

**Implementation rule:** keep depth bounds and sticky failure transitions in the state-machine owner. Once an invalid transition occurs, provide one consistent diagnostic and avoid further unchecked pushes/pops. Give tests both over-nested and prematurely closed input/output sequences.

**Confidence:** High for the design intent because the MR merged and was authored by Guy Harris; narrower for line-level implementation details due to the incomplete captured diff.

## Extend JSON-RPC responses with state-aware fields and exact output tests

A JSON-RPC field that reflects user-configured output state should be produced from the active session/profile, not a fixed default, and should be omitted when the underlying state is unavailable. Changing a response contract must update tests for both initialized and uninitialized application states.

Merged sharkd MR !9763 adds an optional `columns` array to the `status` response by consulting `cfile.cinfo.num_cols` and serializing each active column's `get_column_title(i)` via the standard sharkd JSON array API. The change updates `test/suite_sharkd.py` expectations for the unloaded and loaded-file request sequences and documents the new field in the handler. Unlike the `info` method's column shortcodes, these are user-visible profile header titles.

**API/testing rule:** explicitly specify whether each response key is required or conditional, derive values from actual session state, and test the exact JSON envelope before/after loading a file and after profile changes. A missing key, an empty array, and a default array are not necessarily equivalent client contracts.

**Confidence:** High. Merged master feature with synchronized protocol documentation and request/response regression tests.

## Complete the dumper state transition even when a value has zero bytes

John Thacker's merged master !9126 and paired stable !9130 fix a protobuf-to-JSON crash when a bytes field is empty. After emitting the member name, the JSON dumper still requires a Base64 value. The fix moves json_dumper_begin_base64() before conditional buffer access and json_dumper_end_base64() after it, so both transitions occur even when the buffer is NULL or length zero. The related JSON_DUMPER_FLAGS_NO_DEBUG export avoids turning malformed state transitions into fatal logging errors during fuzzing; it is not a license to ignore the dumper's error state.

**Rule:** treat structured-output writers as state machines: once a key is written, emit the corresponding value representation, including explicit empty values, and balance begin/end operations on every path. Test zero length and failure cases, with fuzzing and diagnostics enabled as appropriate. **Confidence:** extremely high (accepted master plus backport).
