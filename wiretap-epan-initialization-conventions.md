# Wiretap and libwireshark initialization conventions

Merged master MR 5683, authored by João Valverde, reverted an attempt to make epan initialize and clean up Wiretap because the change caused an exit crash. The accepted state keeps Wiretap initialization explicit in callers such as dftest and preserves the ordering requirement that Wiretap be initialized before libwireshark where file-type-dependent handlers need it.

Rule: moving one library's initialization and cleanup under another library changes ownership and ordering. Validate both startup and shutdown behavior across supported frontends before centralizing those calls.

Confidence: very high; the earlier architecture was reverted in merged master because of a concrete exit crash.


## Historical predecessor: !5227 was merged and later reverted

Merged master MR !5227 is the earlier change that moved `wtap_init()` and `wtap_cleanup()` under epan and made Wiretap initialization idempotent. Its rationale was to spare libwireshark clients from explicit Wiretap lifecycle calls when they did not otherwise use Wiretap.

Later merged master !5683 is authoritative: it reverted that ownership change after an exit crash and restored explicit caller ordering. Keep !5227 only as negative lifecycle history showing that successful startup and a cleaner call surface are insufficient proof that ownership is correct.

**Review rule:** when a later merged change explicitly reverts an earlier lifecycle centralization for a concrete teardown failure, record the predecessor as superseded evidence rather than continuing to cite both designs as equally accepted.

**Confidence:** Extremely high for the final ownership rule because both the original merged change and the later merged revert are known; the revert resolves the architectural question.
