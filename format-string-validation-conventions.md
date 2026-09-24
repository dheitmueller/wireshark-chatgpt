# Format String Validation

Merged master MR !10890, authored and merged by Martin Mathieson, adds compiler printf-format annotations to custom variadic formatting helpers across several dissectors.

Implementation rule: a helper that accepts a printf-style format string and variadic arguments should carry the project's format annotation with the correct positions of the format argument and first variadic argument. This preserves compiler checking through the wrapper instead of hiding format/argument mismatches from static diagnostics.

Review rule: after changing such a helper's signature, re-check the annotation indices along with its call sites.

Confidence: High.
