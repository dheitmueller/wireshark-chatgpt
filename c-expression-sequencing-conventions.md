# Wireshark C Expression-Sequencing Conventions

This file records durable C expression and sequencing conventions extracted from accepted upstream Wireshark review. Current upstream source and coding guidance remain authoritative.

## Sequence repeated side effects before a function call

Do not modify the same scalar object more than once across function-call arguments and depend on an apparent left-to-right reading of the call. C does not specify a general evaluation order for function arguments, so such code can have unsequenced side effects and undefined behavior even when it looks compact and obvious.

In merged master MR !24450, John Thacker explicitly rejected setting `hfinfo` twice in the arguments to one function call because the arguments are evaluated in an unspecified order without sequencing. The correction computes or assigns the needed values in separate statements before the call. John also noted that clang-tidy can detect this class of problem.

**Implementation rule:** if two arguments would mutate, assign, increment, or otherwise depend on the same scalar state, establish the required order in separate statements before calling the function. Do not encode ordering assumptions in the argument list.

**Confidence:** Very high. Direct substantive review from John Thacker on a merged master MR, with the correction applied before merge.
