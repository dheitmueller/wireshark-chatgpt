# Masked Boolean Field Conventions

Merged !10359 changes genuine one-bit masked fields from integer value tables to `FT_BOOLEAN` with shared true/false text. Martin Mathieson notes that this conversion applies when the field has a non-zero mask. Merged !10347 independently applies the same representation to MPEG/DVB current/next bits.

**Field rule:** represent genuine one-bit masked protocol flags as Boolean fields. This evidence does not imply that every unmasked scalar with known values 0 and 1 is Boolean.
