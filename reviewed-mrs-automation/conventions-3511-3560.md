# Durable conventions from Wireshark MRs !3511–!3560

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- !3556: preserve observable dependency/version diagnostics when moving implementation between library layers; Guy Harris caught the lost zlib report from `editcap --version`.
- !3538: use Wireshark's built-in heuristic enable/disable mechanism rather than a duplicate protocol preference; a TCP heuristic must also account for segmentation and false-positive risk.
- !3530 plus !3546–!3552 and !3559: maintain generated dissectors through their ASN.1/conformance/template sources and keep checked-in output reproducible.
- !3517 and !3511: keep pcapng option-union member selection inside the owning option layer and use consistent semantic helper names.
- !3526: when only obsolete preference keys remain, register the protocol through the obsolete-preference path instead of retaining an empty active preference surface.
- !3516: CI rules must reflect where dedicated runners actually exist, not only the pipeline event type.
- !3518: candidate scans should break only after the desired semantic match is found.
- !3520: verify that the owning/consumer subsystem exists before allocating an object whose lifetime depends on it.
- !3515: unrelated whitespace cleanup can defeat stable-branch cherry-picks; keep backportable fixes mechanically focused.
- !3528: revalidate draft-era protocol implementations and test-capture provenance against the final published standard.
