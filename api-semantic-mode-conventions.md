# Wireshark API Semantic-Mode Conventions

## Prefer semantic API variants over magic sentinel values

Merged MR !5515, authored by João Valverde, splits regex matching into ws_regex_matches() for NUL-terminated subjects and ws_regex_matches_length() for explicit-length subjects. The change removes WS_REGEX_ZERO_TERMINATED, which had encoded the NUL-terminated mode by passing SIZE_MAX in the length argument.

**Implementation rule:** when two call modes have distinct semantics, prefer separate semantic entry points or an explicit mode over assigning special meaning to an otherwise valid value in a numeric argument domain. This keeps the contract truthful and makes misuse easier to spot at the call site.

**Confidence:** Very high. Merged API cleanup authored by João Valverde.
