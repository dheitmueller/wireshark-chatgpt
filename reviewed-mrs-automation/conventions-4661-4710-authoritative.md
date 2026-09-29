# Authoritative conventions — MRs 4661–4710

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- Reassembly keys must preserve the full protocol identifier and its real uniqueness scope. MR 4677 uses TCP addresses/ports plus the complete 64-bit TCPCL Transfer ID instead of truncating it.
- Third-party implementation types should be hidden behind project-owned semantic interfaces. MR 4679 replaces direct regex-library objects across the display-filter parser, VM, and ftypes with compile/match/pattern/lifetime helpers.
- A cast used to settle a signedness warning needs a range and platform-type proof. Guy Harris's review of MR 4685 and his 4694/4695 follow-ups explicitly justify the `st_blksize` comparison conversion.
- Pointer/integer conversion must honor the conversion API's width. MR 4686 uses `GUINT_TO_POINTER` and `GPOINTER_TO_UINT` for a Follow Stream ID whose consumer is `guint`, avoiding 64-bit assumptions on 32-bit systems.
- Generated build products should be represented with explicit generator targets and dependency edges so independent work can run in parallel. Merged MR 4692 supersedes the platform-specific shortcut in closed MR 4689.
- Compiler upgrades must be validated on specialized build jobs as well as normal compilation. MR 4687 reverts Clang 13 after a repeatable fuzz-builder memory regression.
- Before conditionally removing declarations for a disabled debug facility, verify whether its argument expressions are still compiled. MR 4701 shows that `ws_debug()` still references its arguments when output is disabled.
- Unrelated changes should be split into focused MRs and each MR title should describe its actual content. Stig Bjørlykke and Anders Broman enforce both points in MR 4682.
- Initialize retained protocol state to known sentinels and only copy/promote values that parsing actually established. MR 4688 supplies another merged partial-initialization example.
- Do not use historical spelling cleanups such as MR 4663 as precedent for renaming registered display-filter abbreviations; later, stronger accepted review treats those identifiers as compatibility surfaces.
