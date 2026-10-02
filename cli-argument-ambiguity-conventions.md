# CLI Argument Ambiguity Conventions

## Avoid optional short-option arguments when trailing syntax is meaningful

A short option with an optional argument is a poor fit when the remaining command line also has meaningful positional syntax, such as TShark's trailing capture filter. The parser cannot reliably distinguish an optional option argument from the first token of that positional/filter syntax without changing long-standing command-line meaning.

Merged master MR !1787 adds repeatable `--hexdump <hexoption>` modes instead of turning legacy `-x` into a short option with an optional argument. During review, Guy Harris gave the concrete ambiguity: `tshark -i en0 -x host 192.9.200.1` could interpret `host` either as an `-x` submode or as the beginning of the capture filter. The accepted interface uses a distinct long option whose argument is explicit.

John Thacker's review also caught that the new path must preserve existing stdout behavior even when `-w` writes packets to a file, and that documented timestamp examples must use formats available across the supported platform set.

**CLI grammar rule:** do not retrofit optional arguments onto short options when following argv tokens already carry independent positional/filter semantics. Prefer a long option with a required argument or another syntax whose token boundaries are unambiguous.

**Compatibility rule:** when extending output controls, preserve established output-stream destinations and interaction with other output options unless a change is explicitly documented.

**Documentation/portability rule:** command examples are part of the interface contract; avoid examples that depend on libc parsing/formatting extensions unavailable on supported platforms.

**Confidence:** Extremely high. Merged master feature with direct Guy Harris grammar analysis and detailed John Thacker portability/output review.
