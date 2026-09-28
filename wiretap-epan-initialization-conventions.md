# Wiretap and libwireshark initialization conventions

Merged master MR 5683, authored by João Valverde, reverted an attempt to make epan initialize and clean up Wiretap because the change caused an exit crash. The accepted state keeps Wiretap initialization explicit in callers such as dftest and preserves the ordering requirement that Wiretap be initialized before libwireshark where file-type-dependent handlers need it.

Rule: moving one library's initialization and cleanup under another library changes ownership and ordering. Validate both startup and shutdown behavior across supported frontends before centralizing those calls.

Confidence: very high; the earlier architecture was reverted in merged master because of a concrete exit crash.
