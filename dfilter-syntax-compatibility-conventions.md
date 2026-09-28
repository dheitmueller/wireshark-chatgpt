# Wireshark Display-Filter Syntax Compatibility Conventions

## Deprecate user syntax explicitly and preserve a migration window

Merged !6398, authored by João Valverde, keeps the ~= spelling accepted while registering a deprecation diagnostic that tells users to use !==. Documentation and release notes describe both the deprecation and replacement. The discussion explicitly recognizes that users had already seen different equivalent spellings across 3.4, 3.6, and the developing 3.7/4.0 series.

**Compatibility rule:** when replacing established display-filter syntax, keep the old spelling working for a documented transition period when practical and emit a precise replacement diagnostic rather than silently changing semantics.

Closed !6367 supplies negative design evidence. It proposed =~ as a symbolic alias for contains; João then agreed the spelling was misleading because =~ conventionally suggests a regular-expression operator.

**Language-design rule:** symbolic aliases should communicate the operation users are likely to infer from them. Do not spend compatibility budget on a spelling whose conventional meaning conflicts with the actual operator.

**Evidence weight:** Very high for merged !6398. !6367 is supporting negative evidence only because it was closed unmerged.
