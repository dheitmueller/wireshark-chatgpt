# Wireshark Wiretap File-Detection Conventions

This file records durable conventions for capture/file-format recognition extracted from accepted Wireshark review. Current upstream source and documentation remain authoritative.

## Order file recognizers by confidence so weak heuristics cannot preempt stronger formats

Wiretap file-opening probes are not independent when they are tried sequentially: an early false positive can prevent the correct later recognizer from seeing the file. Detection order is therefore part of the correctness contract, especially for formats whose opener uses only weak structural heuristics rather than a strong magic value or highly specific syntax.

Merged master MR !22467, authored and merged by John Thacker, moves the Ixia IxVeriWave and CAM Inspector openers to the end of `open_info_base[]`. The source already described them as "extremely weak heuristics"; John reported real false positives on otherwise valid JSON. More specific JSON-derived formats remain before the generic JSON reader, while these especially weak format probes are placed after stronger/magic-based recognizers.

**Implementation rule:** when registering or reordering wiretap file recognizers, consider both each recognizer's confidence and interactions with formats that can satisfy the same superficial checks. Put strong magic/signature and narrowly constrained recognizers before broad or weak heuristics; put recognizers known to produce plausible false positives late enough that they cannot steal unrelated files. Treat recognizer order as behavioral logic, not cosmetic table organization.

**Review implication:** a new or modified heuristic opener should be evaluated not only with positive samples of its own format but also against plausible competing/common formats whose contents can accidentally satisfy the heuristic.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, motivated by observed false identification of valid JSON files.
