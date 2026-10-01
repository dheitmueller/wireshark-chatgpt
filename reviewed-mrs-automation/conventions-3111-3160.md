# Convention Synthesis — Wireshark MRs !3111–!3160

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- **Status domains:** !3127 (Guy Harris) keeps a Wiretap return value in `wtap_opttype_return_val` and compares it with `WTAP_OPTTYPE_SUCCESS`. Multivalue statuses should not be stored or tested as booleans.
- **Reassembly identity and nested state:** !3125 (John Thacker) includes all stream context needed to distinguish GSE reassemblies and preserves outer packet identity when nested payload dissectors can change `packet_info`. Incomplete reassemblies are not sent to normal payload dissection.
- **Registration lifecycle:** !3123 (John Thacker), with stable backport !3124, puts heuristic registration inside one-time initialization because preference reapplication can rerun handoff code.
- **Display-filter field identity:** !3117/!3118, with Alexis La Goutte review, show that one filter abbreviation must not be registered for incompatible field types.
- **Capture validity:** !3128 shows that hand-crafted sample captures should be validated against the governing specification and, when practical, an independent/reference implementation.
- **Parser progress:** !3130 (John Thacker) and !3160 terminate malformed-input paths rather than allowing repeated parsing without progress.
- **Formatted I/O:** !3159 records Anders Broman's correction to use `G_GUINT64_FORMAT` for `guint64`; !3135 separately avoids assuming a fixed concrete type for `time_t`.
- **Build defaults:** !3121 (João Valverde) makes LTO opt-in on GCC/Clang after poor measured build-time/performance results. Costly optimizations should not become defaults on theory alone.
- **Capability detection:** !3134 probes whether a dependency actually exports the runtime-version symbol before using it; !3129 gates version APIs on the minimum library version that supplies them.
- **Error cleanup:** !3112 (Guy Harris), plus backports !3113/!3114, frees partially initialized Wiretap private state on an early open error.
- **Canonical decoding/source of truth:** !3111 replaces local APN parsing with `ENC_APN_STR` and updates ASN.1 source together with generated output.
- **Submission details:** !3160 records Pascal Quantin's guidance that Gerrit `Change-Id` trailers were no longer required after the GitLab migration and that issue-closing keywords should express intended closure. !3128 records Graham Bloice's required commit subject form and `ett_` subtree naming convention.

No SMPTE 291/VANC packet type was encountered.
