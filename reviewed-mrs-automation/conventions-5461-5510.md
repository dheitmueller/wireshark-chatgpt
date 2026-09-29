# Durable conventions from MRs !5461-!5510

Corpus: `ddcaa22b51c68f594e425a23388c3a2086813054`

- !5509: concrete text encodings stand alone; do not combine a neutral encoding marker with a real character encoding.
- !5498: match the fixed-width integer format-macro family to formatted input versus output.
- !5497 and !5503: compile supported platform-conditional paths after broad API/header migrations.
- !5473: prefer the standard C feature when the project baseline provides it, while retaining a compatible C++ path in shared headers.
- !5484: CLI and GUI text-import surfaces should share one parser/conversion engine.
- !5465 and !5466: keep Wiretap timestamp precision and serialized interface metadata semantically consistent.
- !5463, !5464, and !5479: keep broad optional-feature-off CI coverage and remove redundant narrower variants.
- !5461: use Wireshark's common checksum presentation helper when its model fits.

Closed !5475 is workflow corroboration for clean backport history. Open !5489 is retained only as provisional design history and is not treated as accepted architecture.
