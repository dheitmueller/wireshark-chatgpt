# WSLua range-coordinate conventions

## Bounded views use view-relative coordinates

Merged master MR !275 fixes `TvbRange:raw()` after its optional offset and length were interpreted against the original TVB instead of the current range. The accepted implementation validates against `tvbr->len`, computes the default remaining length inside the range, and adds `tvbr->offset` only at the final backing-TVBuff access. Stig Bjørlykke explicitly requested `raw(offset)` and `raw(offset,length)` tests; the MR added coverage for whole ranges and nested offset/length combinations.

**Implementation rule:** offsets exposed by a bounded-view API are relative to that view unless the API explicitly says otherwise. Validate in the view's coordinate domain and translate to backing-storage coordinates only where the underlying read occurs.

**Testing rule:** exercise every optional offset/length form on both full objects and sliced views, including boundary and out-of-range cases.

**Confidence:** Very high. Merged master correctness fix with direct maintainer review and expanded regression tests.

## Normalize tolerated syntax before calling stricter helpers

Merged master MR !276 makes WSLua `ByteArray:base64_decode()` accept unpadded Base64 by adding the necessary `=` padding to a private buffer before calling GLib's decoder. Both padded and unpadded cases are tested.

**Implementation rule:** when a scripting API intentionally accepts syntax broader than a lower-level library helper, normalize at the language-binding boundary rather than duplicating the underlying decoder.

**Confidence:** High. Merged master fix with regression tests.
