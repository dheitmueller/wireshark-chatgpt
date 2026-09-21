# Wireshark Authoritative-Source Preservation Conventions

This file records durable review conventions for source text copied from external protocol specifications or other authoritative upstream material. Current upstream source and project policy remain authoritative.

## Distinguish verbatim authoritative material from Wireshark-maintained wrappers before cleanup

Files used by generated dissectors can contain both text copied from an external specification and Wireshark-owned preambles, annotations, or glue. Spelling, whitespace, and style cleanup that is appropriate in the maintained portions can be undesirable in the copied portions because it creates divergence from the authoritative source and makes future synchronization harder.

During merged MR !14353, Guy Harris explicitly asked whether the touched ASN.1 files were copies of official protocol specifications and noted that, if so, Wireshark might not want to "fix" them. The contributor established that the substantive edits were in a Wireshark-specific header rather than copied specification material. Martin Mathieson then accepted the change on that basis and agreed that trailing whitespace could be removed where the files were already being touched.

**Implementation rule:** before applying spelling, formatting, or whitespace cleanup to an imported ASN.1/specification file, identify which regions are authoritative external text and which are maintained by Wireshark. Keep purely cosmetic project-local edits out of verbatim imported regions unless there is a deliberate reason to carry a local deviation.

**Tooling rule:** automated spelling/formatting checks should account for authoritative imported material. Prefer scoped exclusions or checks limited to Wireshark-maintained regions over forcing copied protocol text to satisfy project-local prose/style rules.

**Review rule:** when a diff touches imported source, ask whether each change belongs in the external authoritative input, the Wireshark-owned wrapper/preamble, or the generator. Avoid accepting a convenient cleanup in the wrong source-of-truth layer.

**Confidence:** Very high. The distinction was raised directly by Guy Harris during review of a merged MR and the accepted resolution was confirmed by Martin Mathieson after verifying that the modified material was Wireshark-owned rather than copied from the specifications.
