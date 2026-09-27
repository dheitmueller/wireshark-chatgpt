# Wireshark Capture Option Length-Budget Conventions

This file records durable conventions for validating length-limited capture-file options and their payloads. Current upstream source and format specifications remain authoritative.

## Budget fixed option overhead before validating variable payload length

If an on-disk length field describes the entire option body, the maximum variable payload is smaller than the field's numeric maximum whenever the option body also contains fixed discriminator, type, version, or other overhead bytes. Size-counting and write paths must enforce the same effective bound.

Merged master MR !15322 was authored and merged by Guy Harris and fixes pcapng `IDB_FILTER` sizing. The pcapng option body length is a 16-bit quantity, but the first byte of the filter option is the filter type; therefore the filter string must be shorter than `UINT16_MAX - 1`, not merely shorter than `UINT16_MAX`. The actual writer already used the correct bound, while the code that counted the option length used the looser one; the accepted fix makes the two paths agree. Stable-branch backports !15323, !15324, and !15325 corroborate the correctness importance of the change.

**Implementation rule:** derive the variable-payload limit from the serialized container budget: `encoded maximum - fixed body overhead`, with padding/terminators included according to the format. Do not compare only the user payload against the raw maximum of the enclosing length field.

**Consistency rule:** any preflight/counting routine, size calculator, validation routine, and actual writer must use the same effective bound. A writer that rejects data after a counter has accepted it can produce incorrect allocation/length calculations even if the final serialization check is correct.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by Guy Harris, with three accepted stable-branch backports and an explicit explanation of the one-byte option-body overhead.

## Give alignment padding exactly one owner and keep option-body sizing reusable

A helper that reports the serialized size of an individual option should have one unambiguous contract for whether alignment padding is included. If the enclosing block-size calculator and the physical write path already add alignment, the option-specific helper should report only the option body bytes. That same body-size value can then be used both for aggregate sizing and for the option header's encoded `value_len`, avoiding parallel calculations that can drift.

Merged master MR !15263, authored and merged by John Thacker, fixes several pcapng hash/verdict option bugs: missing type-octet accounting, an incorrect option type, fixed-length hash overrun handling, and inconsistent sizing. During review, Guy Harris explicitly asked whether any of the `pcapng_compute_..._option_size()` routines should be adding padding at all. Merged follow-up !15271, again authored by John Thacker, generalizes the answer: option sizers stop calculating padding because `compute_block_options_size()` and the option-writing routines already own that responsibility. The writer can therefore reuse the same option-size helper for the option header length as well as the total block-size calculation.

**Implementation rule:** define serialized-size helpers in terms of one representation layer. If the helper returns option-body length, do not include outer alignment there; let exactly one enclosing layer add padding. Reuse that helper wherever the same logical length must be emitted or precomputed rather than re-deriving the value independently.

**Review rule:** when changing a capture writer's size arithmetic, trace all three uses together: the individual option header length, the aggregate block length, and bytes physically written including alignment. A change is incomplete if those paths can compute the same logical option from different formulas.

**Confidence:** Extremely high. Two merged master fixes authored by John Thacker, with Guy Harris directly challenging the padding ownership and the accepted follow-up generalizing the design across option sizers.

## Validate format limits at user-facing ingress and again at the serialization boundary

Merged master MR !7614, authored by Guy Harris, checks pcapng's 65535-byte comment-option limit in Wireshark and `editcap` before accepting the user's comment. Rejecting the value there lets the application explain immediately why the requested comment cannot be saved. Guy's MR description also explicitly says libwiretap should perform its own check so callers that bypass those frontends cannot create an invalid option.

The same description notes that 65535 is a pcapng option limit and that another capture format could in principle allow a larger comment.

**Layering rule:** enforce a hard serialized-format limit at the earliest user-facing boundary that can produce an actionable error, but also enforce the invariant in the core writer/library that ultimately owns the representation.

**Scope rule:** bind the limit to the file format or serialized object that defines it. Do not turn a pcapng-specific encoding bound into an unconditional application-wide cap unless all supported representations share that bound.

**Confidence:** Extremely high. Merged master fix authored by Guy Harris with release-3.6 and release-3.4 backports !7616 and !7620.
