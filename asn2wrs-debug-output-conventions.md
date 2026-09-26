# ASN2WRS Debug-Output Conventions

Merged MRs !8955 and !8959 make asn2wrs line directives opt-in through the common build setting `ENABLE_DEBUG_A2W` instead of making them normal generated output or a per-dissector choice.

**Rule:** generator-only debugging annotations should have one project-wide control and a stable normal-build default. When generator output policy changes, update the common invocation and regenerate checked-in derivatives together.

**Confidence:** High; two adjacent merged master changes.
