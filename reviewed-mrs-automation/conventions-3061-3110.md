# Durable Convention Synthesis — !3061–!3110

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Ownership and cleanup

- Delay allocation until prerequisites are known, and treat ownership transfer as explicit state. Guy Harris's merged master !3083 allocates RTP stream IDs only after required dissection state exists, then frees any ID not actually transferred to the caller's result container.
- Centralize failure cleanup rather than duplicating it before a shared epilogue. In merged Guy Harris !3079, the common failure path owns temporary-file removal; the branch that jumps there no longer unlinks first. The same change preserves the primary export failure instead of replacing it with a secondary close error.
- Audit every exit path according to the API that produced the resource. !3103, !3100, !3097, !3084, !3080, !3076, !3069, !3066, and !3063 form a strong Guy Harris-authored series covering heap buffers, TVBuff ownership transfer, GLib string vectors, temporary pathnames, error strings, directory handles, and recursive-loader paths.

## Dissector and API structure

- Carrier code should establish a bounded child TVBuff, and a reusable child protocol should use the standard dissector signature. John Thacker's merged !3093 makes DVB-S2 GSE a bounded `dissector_t`-shaped decoder instead of passing parent offset/length parameters.
- Keep the typed value in the field and put reusable presentation in field metadata. Guy Harris's merged !3073 records the DOF session key through the bytes-with-length API and uses `SEP_COLON` in registration instead of allocating an ad hoc display string.
- A multi-valued return code is not a Boolean. Guy Harris's merged !3063 explicitly compares the protobuf loader result with zero and notes that, if callers only need binary status, the API itself could instead be Boolean. This corroborates the later !3127 status-domain rule.

## Formatting, reentrancy, and portability

- Mark printf-like variadic helpers so the compiler can validate calls. Guy Harris's !3075 adds `G_GNUC_PRINTF` and immediately exposes a real missing argument.
- Do not use mutable static storage for per-call formatting scratch. The same !3075 moves the buffer to automatic storage for thread safety.
- Cross-platform CI is part of type and format validation. In merged !3072, Windows exposed narrowing of a 64-bit sequence identifier and macOS exposed non-portable `guint64` printf formatting; Gerald Combs specifically directed use of `G_GUINT64_FORMAT`.

## Text and wire representation

- Parse structural octets before decoding character payload. Guy Harris's merged !3062 parses APN label-length bytes directly and decodes only label contents, with bounds checks and replacement handling. A generic ASCII decoder must not transform framing bytes.
- User-visible terminology should follow protocol semantic roles. !3061 changes Zigbee route-reply Info labels from generic source/destination wording to the specification's Responder/Originator terminology.

## Weighting notes

- !3106 is closed and explicitly superseded by later !3837; use it only as submission/review history.
- !3095 merged an earlier policy of broadly enabling LTO/IPO when CMake reports support. The later reviewed merged !3121 is the stronger/current policy precedent because it deliberately makes LTO opt-in after unfavorable project experience.
- Stable-branch duplicates corroborate the corresponding master fixes but do not outweigh the master changes.
