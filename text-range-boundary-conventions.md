# Wireshark Text Range-Boundary Conventions

Merged master MR !5449, authored by John Thacker, fixes a text-import timestamp bug where the parser received an end pointer one byte short.

**Rule:** for APIs that take a half-open text range, derive the start and end from the same base object and make the end point exactly one byte past the final input character. Avoid mixed-base pointer-and-length arithmetic.

**Testing rule:** include inputs whose last character changes the parsed value, such as timestamps ending in a timezone or fractional-second digit.

**Confidence:** Very high; merged master correctness fix authored by John Thacker.
