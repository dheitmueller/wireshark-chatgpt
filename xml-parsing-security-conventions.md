# XML Parsing Security Conventions

This file records durable XML parser-security conventions extracted from upstream Wireshark merge requests. Current upstream source remains authoritative; these notes are a curated review cache.

## Do not enable external-entity substitution for untrusted XML

XML content obtained from captures, trace/configuration payloads, or other externally supplied data must be parsed with options that do not expand external entities. In libxml2 specifically, `XML_PARSE_NOENT` enables entity substitution and can turn an otherwise local parse into an XML External Entity (XXE) attack surface.

Merged MR !23767 fixes QCDIAG XML parsing after Coverity flagged `XML_PARSE_NOENT` as `UNSAFE_XML_PARSE_CONFIG`. The accepted implementation removes that option rather than attempting to sanitize entity references after expansion. The MR included representative QCDIAG captures and was ultimately approved and merged by Anders Broman.

**Implementation rule:** treat parser feature flags as part of the input trust boundary. For packet-derived or otherwise untrusted XML, keep external entity resolution/substitution disabled unless there is a narrowly justified requirement and an independently safe resolver policy.

**Review rule:** when static analysis identifies an unsafe parser configuration, address the dangerous parser capability itself rather than merely suppressing the warning or validating data after the parser has already performed external resolution.

**Confidence:** Very high. Merged master security hardening with an explicit Coverity XXE finding and maintainer approval.
