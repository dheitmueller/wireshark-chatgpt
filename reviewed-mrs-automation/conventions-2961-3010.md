# Convention synthesis — !2961–!3010

- !2966/!2991/!2998/!2999/!3010 (with !2974/!2967/!2978/!2990 corroboration): when an enclosing generated ASN.1 structure supplies semantic context to a generic child field, carry that context in packet/private state, consume it once, and reset it immediately. Nested children must not overwrite a more-specific outer semantic context such as RAI with LAI.
- !2997: reassembly completion/classification must use metadata belonging to the target reassembly identity. Do not consult the arbitrary element left in an iteration variable after scanning a mixed list.
- !3004/!3008, with closed !3009 as diagnostic history: express Qt AUTOGEN requirements at the target level for multi-config generators. Remove a global CMake workaround only after validating the affected and unaffected CMake/generator combinations.
- !2996 (Guy Harris): the MR title and commit subject should say what the change is; put the bug-closing reference later. Amending the Git commit does not update the GitLab MR text, and stale pre-rebase fork pipelines are not evidence about the current MR.
- !2993: values retained in long-lived wmem-owned structures should use an allocator/lifetime compatible with the container's semantic lifetime; targeted ASAN test runs are effective at finding mismatches.
- !2985: weak heuristics should be disabled by default, and recognition must check captured-byte availability before reading bytes needed by the heuristic.
- !2983: fuzz substantial reusable decoder APIs before merge, especially loops driven by packet counts/lengths. Positive RFC-derived examples do not cover malformed/truncated inputs. Later !10376 is stronger authority for the COSE media_type data-context correction.
- !2980: do not force an unusual wire integer into a generic 64-bit varint API when the protocol's representable domain materially exceeds that API's contract.
- !2976 is down-weighted because the alternative UAT design later merged as !3597; !2994 is superseded by !2601; !2981 is an earlier superseded IEC submission. Closed work is not treated as implementation precedent.

No SMPTE 291/VANC packet type was encountered.
