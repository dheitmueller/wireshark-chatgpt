# Wireshark `errno` and Sentinel API Contract Conventions

This file records durable conventions for APIs whose return domain overlaps an error sentinel or whose implementation calls helpers that may modify `errno`. Current upstream interfaces remain authoritative.

## Do not treat ambient `errno` as an independent success/failure result

`errno` is process/thread-local side-channel state, not a durable record of whether some earlier operation succeeded. A successful call can leave a nonzero value behind, and an intermediate helper can overwrite it. Callers should therefore determine success from the API's documented return contract and consult `errno` only when that contract explicitly says it is meaningful for that return.

Merged master MR !12002, authored by Guy Harris, removes a test that expected `errno == 0` after parsing had already been established as successful. The MR states the core reason directly: at that point there is no guarantee that `errno` is zero, so testing it can turn unrelated stale side-channel state into a false parse failure.

Merged master MR !11998, also authored by Guy Harris, applies the stronger design form to `tvb_get_string_bytes()` and `tvb_get_string_time()`: conversion failure is represented by a `NULL` return instead of `errno`. The MR explains that intermediate calls can overwrite `errno`, and that assigning project-specific meanings to a finite system errno namespace would make future error distinctions awkward. The accepted API therefore carries success/failure in its direct return channel and reserves expert diagnostics for the semantic conversion error.

**API rule:** prefer an explicit return-status/sentinel contract over ambient `errno` for parser and conversion APIs. Once an API has reported success, do not subsequently reject that result merely because `errno` happens to be nonzero.

**Review rule:** whenever code reads `errno`, verify that the immediately relevant API documents `errno` as meaningful for the specific return value being handled. A later read after unrelated calls, or an unconditional `errno == 0` success check, is suspect.

**Confidence:** Extremely high. Both are merged master changes authored by Guy Harris; !11998 changes the API itself and !12002 removes an incorrect caller-side `errno` assumption.

## If a legitimate result collides with an error sentinel, make the discriminator part of the API contract

Sometimes a platform-style API must retain a return value that is also valid data. In that case, the return value alone is insufficient and the wrapper must normalize a second discriminator deliberately rather than relying on whatever `errno` a platform implementation happened to leave behind.

Merged master MR !11973, authored by Guy Harris, fixes `mktime_utc()` because `(time_t)-1` is both a conventional failure result and the valid Unix timestamp for 1969-12-31 23:59:59 UTC. The accepted wrapper specifies the pair as the contract: on error it returns `-1` and sets `errno` to `EINVAL`; on success it sets `errno` to zero, including when the legitimate timestamp itself is `-1`. This also normalizes differing `timegm()` behaviors across supported platforms.

The immediately preceding merged !11971 had preserved the caller's prior `errno` around `timegm()` because successful implementations on macOS/FreeBSD could modify it. !11973 supersedes that behavior with a stronger explicit wrapper contract once the valid-`-1` ambiguity is considered. Weight the later contract more heavily.

**API rule:** when every value in the primary return domain can be legitimate, define an unambiguous success/failure discriminator and normalize it inside the wrapper. Do not infer failure from a sentinel value that can also represent valid data.

**Portability rule:** wrappers around platform functions should present one project-level contract even when individual libc implementations differ in how they modify `errno` on success. Callers should not need platform-specific knowledge to interpret the result.

**Testing rule:** include the exact legitimate value that collides with the traditional error sentinel, plus ordinary success and genuine failure cases. For time conversion this means explicitly testing the second immediately before the Unix epoch, not just positive timestamps.

**Confidence:** Extremely high. !11973 is a merged master API-contract correction authored by Guy Harris and directly documents the ambiguous `-1` value and normalized `errno` behavior.