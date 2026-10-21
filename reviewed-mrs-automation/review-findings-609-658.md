# Review findings: Wireshark MRs !609-!658

## High-value findings

- !658 and !656: Guy Harris made Wiretap interface metadata incremental. New interface descriptions can appear after open and even near end-of-file, so consumers must drain newly discovered metadata as reading progresses.
- !649 and !650: Guy Harris replaced convenient format-name tests with the semantic properties actually needed: interface-ID capability and packet encapsulation.
- !651: John Thacker fixed SOCKS TCP desegmentation by looking up conversation state only after the effective port was changed for recursive TCP processing.
- !655: WEP state is created only after a key successfully processes a packet; a dedicated regression capture and test were added.
- !632 and !641-!643: parser-generator syntax is selected according to supported Bison/Berkeley YACC behavior. Guy Harris also separated warning cleanup from the distinct MSVC correctness fix.
- !636: Guy Harris distinguished protocol identity from dissector registration identity; user-visible field names and values must describe the same semantic namespace.
- !637: John Thacker moved reusable EUC-KR and GB18030 handling into common Wireshark text-decoding APIs.
- !615, with !645, !652, !631, !630 and !633: typed-item length checks expose real field metadata problems and are useful for commit-scoped validation, but results still require protocol-aware review.
- !620: Alexis La Goutte required a transformed DNS NSEC3 textual value to be marked generated.
- !621: Guy Harris removed a 32-bit narrowing step from relative-time formatting.
- !618: John Thacker fixed an explicit-length string-buffer append so a leading zero byte does not imply an empty value.
- !616 and !646: a PDML performance change and a separate UTF-8 output-behavior change were intentionally reviewed and landed separately.

## Closed proposals

!657 was superseded by merged !658. !639 was closed after Gerald Combs preferred fixing deprecated Qt APIs over suppressing their warnings. !629 contains maintainer guidance favoring authoritative, reproducible registry sources. !617 contains Pascal Quantin's guidance to prefer explicit or learned PDCP-NR configuration over a weak payload heuristic. !610 was rejected as a production C dissector added only as an example.

No SMPTE ST 291/VANC packet type was encountered.
