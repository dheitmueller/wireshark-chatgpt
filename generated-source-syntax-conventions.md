# Wireshark Generated-Source Syntax Conventions

This file records durable conventions for output that is intended to be valid source code or source-like text. Current upstream source remains authoritative.

## Keep arbitrary payload text from changing the host language's lexical structure

When Wireshark exports captured or otherwise untrusted bytes as source code, text that is meant to be merely decorative still participates in the target language's lexical and preprocessing rules. Sanitizing for printable characters is not enough if a payload character can escape a newline, terminate a comment/string, or otherwise change how later generated text is parsed.

Merged master MR !14204, authored by John Thacker, fixes Export Packet Bytes as a C array. The exporter appended an ASCII rendering in a `//` comment. If the final rendered byte on a line was a backslash, C preprocessing performed backslash-newline splicing before comment removal, causing the next generated source line to become part of the comment. Adding trailing whitespace was rejected as an unreliable workaround because GCC and Clang still recognize a backslash followed by whitespace and newline as a continuation (with a warning). The accepted fix encloses the ASCII rendering with explicit delimiters so arbitrary packet text cannot end the generated line with the escape character.

**Implementation rule:** treat generated comments, strings, identifiers, and line endings as syntax, not presentation. Encode or delimit arbitrary data so no possible input byte can terminate, continue, or otherwise alter the surrounding language construct. Do not rely on whitespace tricks whose behavior is compiler-dependent or accepted only as an extension.

**Review/testing rule:** include adversarial payload endings and delimiters: trailing backslash, comment markers, quotes, control characters, and data at exact line-wrap boundaries. Compile or parse the generated artifact with the supported toolchains when practical; visual inspection of the exported text is insufficient.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker with the C preprocessing failure mode and rejected whitespace workaround documented directly in the MR.