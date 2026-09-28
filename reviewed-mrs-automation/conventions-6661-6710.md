# Durable conventions from Wireshark MRs !6661-!6710

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- **Balanced recursion state (!6700):** once a dissector increments recursion/protocol depth, every exit path must perform the matching decrement. Prefer a shared cleanup path or scoped helper.
- **Confidence-ordered wiretap probes (!6699):** weak heuristic readers can steal unrelated files. Run strong magic/signature or narrowly constrained recognizers first. Guy Harris explicitly highlighted the weakness of the IxVeriWave heuristic after it matched a systemd journal.
- **Canonical display-filter semantics (!6695):** new builtins should reuse the language's field types and ordinary comparison semantics. Variadic language features need end-to-end compiler/VM, documentation, release-note, and repeated-field test coverage.
- **Semantic identifier resolution (!6686):** use the registered-field/parser resolver to distinguish field references from other language constructs; punctuation is not a reliable semantic test.
- **Source-span diagnostics (!6665, !6670, !6678):** carry token ranges from scanner/AST through compilation and model an unavailable span explicitly rather than reconstructing positions late.
- **Stable TCP reassembly identity (!6666, !6685):** frame and protocol-layer identity matter across passes. If out-of-order data completes a PDU prefix while a later PDU remains incomplete, split reassembly state at the consumed boundary instead of reprocessing the prefix on another frame/layer.
- **Robust packet columns (!6663):** Guy Harris recommended writing stable base Info text before packet reads that might fail, then appending optional decoded detail afterward with append-style column APIs.
- **Script compatibility (!6680, !6698):** when native menu/stat identifiers are renamed, preserve deprecated Lua aliases where practical and update generated binding documentation.
- **Provisioning correctness (!6673-!6675):** setup scripts used to build reusable images must report partial setup as failure and handle unset/optional arguments deliberately.
- **Wide arithmetic (!6688):** widen operands before large unit multiplications; casting the result after a narrow intermediate overflows is too late.
- **Dependency-major gating (!6661, !6662):** detect and reject incompatible major versions explicitly until their new API is supported.
- **Lower-weight workflow evidence:** closed !6692 supports splitting unrelated changes into separate MRs; closed !6682 supports submitting from a topic branch rather than fork master.
