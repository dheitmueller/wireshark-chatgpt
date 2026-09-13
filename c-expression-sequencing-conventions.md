# Wireshark C Expression-Sequencing Conventions

This file records durable C expression and sequencing conventions extracted from accepted upstream Wireshark review. Current upstream source and coding guidance remain authoritative.

## Sequence repeated side effects before a function call

Do not modify the same scalar object more than once across function-call arguments and depend on an apparent left-to-right reading of the call. C does not specify a general evaluation order for function arguments, so such code can have unsequenced side effects and undefined behavior even when it looks compact and obvious.

In merged master MR !24450, John Thacker explicitly rejected setting `hfinfo` twice in the arguments to one function call because the arguments are evaluated in an unspecified order without sequencing. The correction computes or assigns the needed values in separate statements before the call. John also noted that clang-tidy can detect this class of problem.

**Implementation rule:** if two arguments would mutate, assign, increment, or otherwise depend on the same scalar state, establish the required order in separate statements before calling the function. Do not encode ordering assumptions in the argument list.

**Confidence:** Very high. Direct substantive review from John Thacker on a merged master MR, with the correction applied before merge.

## Separate validation from a call when another argument depends on that validation happening first

Unspecified function-argument evaluation order is also dangerous when the arguments do not modify the same object. If one argument invokes a bounds-checking or exception-raising accessor and another argument performs arithmetic that is only safe after that validation, putting both expressions in one call does not establish the required order. The compiler may evaluate the arithmetic first.

Merged master MR !21565, authored and merged by John Thacker after OSS-Fuzz found signed-overflow undefined behavior in the Kafka dissector, explicitly split `tvb_get_ptr()` out of a protocol-tree call. The TVBuff access is capable of throwing on an invalid/overflowing packet-derived range, but the other arguments contained length arithmetic that could overflow before the accessor was evaluated. The accepted fix performs the TVBuff access in a separate statement, then makes the call only after validation has completed.

**Implementation rule:** when correctness relies on one operation validating, throwing, clamping, or otherwise establishing an invariant before another expression is evaluated, make that ordering explicit with separate statements. Do not use placement in a function argument list as a sequencing mechanism.

**Confidence:** Extremely high. Merged master OSS-Fuzz fix authored and merged by John Thacker, with the C evaluation-order issue called out directly in the change rationale.