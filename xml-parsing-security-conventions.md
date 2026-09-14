# XML Parsing Security Conventions

This file records durable XML parser-security conventions extracted from upstream Wireshark merge requests. Current upstream source remains authoritative; these notes are a curated review cache.

## Do not enable external-entity substitution for untrusted XML

XML content obtained from captures, trace/configuration payloads, or other externally supplied data must be parsed with options that do not expand external entities. In libxml2 specifically, `XML_PARSE_NOENT` enables entity substitution and can turn an otherwise local parse into an XML External Entity (XXE) attack surface.

Merged MR !23767 fixes QCDIAG XML parsing after Coverity flagged `XML_PARSE_NOENT` as `UNSAFE_XML_PARSE_CONFIG`. The accepted implementation removes that option rather than attempting to sanitize entity references after expansion. The MR included representative QCDIAG captures and was ultimately approved and merged by Anders Broman.

**Implementation rule:** treat parser feature flags as part of the input trust boundary. For packet-derived or otherwise untrusted XML, keep external entity resolution/substitution disabled unless there is a narrowly justified requirement and an independently safe resolver policy.

**Review rule:** when static analysis identifies an unsafe parser configuration, address the dangerous parser capability itself rather than merely suppressing the warning or validating data after the parser has already performed external resolution.

**Confidence:** Very high. Merged master security hardening with an explicit Coverity XXE finding and maintainer approval.

## When local external entities are required, explicitly prohibit network retrieval

Some trusted project-controlled XML formats legitimately use external entities as part of their on-disk schema. In that case, disabling all entity processing can break required functionality, but leaving the parser free to retrieve arbitrary network resources is broader than the format requires. Preserve the narrow local-file behavior and explicitly disable network access.

Merged MR !20074 replaces the Flex-based Diameter dictionary parser with libxml2. The dictionary format depends on local external entities, so simply applying `XML_PARSE_NO_XXE` would not preserve existing behavior. During review, John Thacker recommended `XML_PARSE_NONET`: Wireshark needs the local entity includes, but it has never needed to fetch external entities over the network. The MR also validated the replacement parser by comparing the old and new dictionary output and exposed path/working-directory issues through CI builds using unusual path names.

**Implementation rule:** separate the *semantic feature* the format needs from the *transport capability* the parser happens to offer. If local includes/entities are required, configure a resolver policy that permits only the required local resources and rejects network retrieval rather than enabling unrestricted external resolution.

**Migration/testing rule:** parser-backend replacements should demonstrate semantic equivalence against the previous parser and should be exercised in CI environments that stress resource lookup assumptions, such as nontrivial build paths and working directories.

**Confidence:** Very high. Merged master parser replacement with direct John Thacker security guidance and CI-driven fixes.