# Convention synthesis — !3011–!3060

- !3053: Give reusable child dissectors a bounded TVBuff and the standard dissector signature when practical.
- !3045/!3047: Centralize recurring APN/DNN wire-string decoding in a shared TVBuff/proto encoding helper and migrate callers.
- !3044: API names should make the difference between optional developer assertions and always-active invariant checks clear; Guy Harris explicitly challenged the historical naming.
- !3038: Parser progress and destination-array capacity are separate invariants; consume the encoded structure safely while bounding writes.
- !3034/!3036: Preserve structured internal data instead of serializing to text and parsing it back into structure.
- !3030/!3029: STARTTLS-style state must transition on the response/event that consumes the pending request, not merely on the next frame.
- !3026: Compile-time dependency/version information and runtime loaded-library/version information answer different questions and should be reported separately.
- !3025: Fuzz fixes must validate the actual failing contract; ancillary dissector context can be absent independently of TVBuff length.
- !3020: Version-dependent optional fields still need explicit version gating when the protocol standard restricts them.
- !3014: Public exported headers should not pull in implementation-only dependencies as incidental transitive includes.

Closed !3041, !3027, and !3013 were down-weighted in favor of their merged successors or later accepted work.
