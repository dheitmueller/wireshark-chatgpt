# Automated MR review ledger: !19740-!19789

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Reviewed exactly 50 previously-unreviewed merge requests, selected by reconstructing the already-reviewed set from `reviewed-mrs.md` and all available files in `reviewed-mrs-automation/`, while preserving/counting the historical !17571-!17620 batch. The corpus revision was unchanged from the preceding run, so no newly-populated higher-numbered holes displaced the descending frontier.

Reviewed MRs:

!19789 !19788 !19787 !19786 !19785 !19784 !19783 !19782 !19781 !19780
!19779 !19778 !19777 !19776 !19775 !19774 !19773 !19772 !19771 !19770
!19769 !19768 !19767 !19766 !19765 !19764 !19763 !19762 !19761 !19760
!19759 !19758 !19757 !19756 !19755 !19754 !19753 !19752 !19751 !19750
!19749 !19748 !19747 !19746 !19745 !19744 !19743 !19742 !19741 !19740

## Durable findings

The batch was weighted toward merged changes and authoritative maintainer-authored/reviewed work. The strongest architectural evidence was !19761, authored and merged by Guy Harris: Netflix pcapng custom block/option handling was moved out of core `pcapng.c` into its own module, explicitly as a step toward allowing custom blocks/options without modifying core pcapng code and eventually supporting them from plugins. This directly corroborates the existing notebook convention that common pcapng infrastructure should identify and route extension data while extension modules own format-specific semantics.

!19789 is a compact portability example: use of CMake's `IS_WRITABLE` path predicate is version-gated because it is unavailable before CMake 3.29; older supported CMake versions retain a reduced directory check rather than invoking unsupported syntax. This corroborates existing build/compatibility guidance rather than requiring a new standalone convention.

No new notebook convention was added in this run because the strongest durable findings reinforce conventions already captured in the notebook rather than adding a materially distinct rule. Low-signal dependency, build, documentation, backport, cosmetic, and narrow protocol-specific changes were reviewed without manufacturing generalized rules from them.
