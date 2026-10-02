# Wireshark Platform-Specific Checker Conventions

## Exclude platform-only source narrowly; do not disable the checker

Merged MR !1653 added Windows-only ETW live-capture code. `validate-clang-check.sh` ran on Linux and failed because ETW system headers were unavailable there. Guy Harris explicitly directed the contributor to extend the checker's existing exclusion list for the Windows-only ETW translation units rather than skipping the checker.

**Tooling rule:** when a standalone checker cannot analyze source that only builds on another platform, make the exception explicit and as narrow as possible. Keep the check active for every source file that can be analyzed in the checker environment.

**Maintenance rule:** checker scripts are shared project infrastructure. If the script changed upstream while an MR was in review, rebase first and adapt the exception to the current implementation.

**Confidence:** Extremely high. Direct Guy Harris review on a merged MR, with the requested checker behavior adopted before merge.
