# Wireshark Display-Filter Syntax Compatibility Conventions

## Deprecate user syntax explicitly and preserve a migration window

Merged !6398, authored by João Valverde, keeps the ~= spelling accepted while registering a deprecation diagnostic that tells users to use !==. Documentation and release notes describe both the deprecation and replacement. The discussion explicitly recognizes that users had already seen different equivalent spellings across 3.4, 3.6, and the developing 3.7/4.0 series.

**Compatibility rule:** when replacing established display-filter syntax, keep the old spelling working for a documented transition period when practical and emit a precise replacement diagnostic rather than silently changing semantics.

Closed !6367 supplies negative design evidence. It proposed =~ as a symbolic alias for contains; João then agreed the spelling was misleading because =~ conventionally suggests a regular-expression operator.

**Language-design rule:** symbolic aliases should communicate the operation users are likely to infer from them. Do not spend compatibility budget on a spelling whose conventional meaning conflicts with the actual operator.

**Evidence weight:** Very high for merged !6398. !6367 is supporting negative evidence only because it was closed unmerged.


## Reserve separator-free literal space when another useful domain may need it

Merged 5670, authored by João Valverde, makes ISO 8601 the preferred display-filter absolute-time representation but deliberately retains legacy input formats. During review, John Thacker points out that ISO 8601 basic date-time syntax without separators can consist entirely of digits and therefore collide with a future numeric Unix-epoch literal. João agrees that epoch syntax is more valuable than consuming the ambiguous numeric spelling. Merged 5689 is the accepted follow-up: filter time literals use the separated ISO 8601 form rather than the auto-detected form.

**Language-design rule:** when two plausible literal domains overlap lexically, do not assign the ambiguous spelling merely because an existing parser can accept it. Prefer an unambiguous spelling for the current feature and preserve unused lexical space for future language evolution.

**Compatibility rule:** changing the preferred displayed form does not require gratuitously breaking old accepted input. MR 5670 keeps legacy absolute-time forms parseable while changing the canonical representation.

**Confidence:** Very high. Both master changes merged; the ambiguity and future-compatibility rationale comes directly from substantive John Thacker and João Valverde review.
