# Generated-code conventions from !10313-!10362

Guy Harris directly corrected three cases where generated dissector output had been edited without updating the authoritative generator input: !10330 (SPNEGO), !10341 (ATN-ULCS), and !10343 (ILP). The merged follow-ups !10353, !10351, and !10352 update the corresponding ASN.1 template or conformance source. Stable backports !10354-!10357 preserve the same source/output parity.

**Durable rule:** for generated dissectors, make the semantic fix in the template, conformance file, IDL, or other authoritative input, regenerate the checked-in output, and submit both layers together when the generated artifact is version-controlled.

**Confidence:** extremely high because the same correction was made repeatedly by Guy Harris and followed by merged source-level fixes.
