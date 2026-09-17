# Automated Wireshark MR review: !17659-!17708

Reviewed against corpus commit `a3b98766b09f6822e4f2d38d2925205c0ee92383`.

Selection was made after consulting the persistent `reviewed-mrs.md` ledger and all existing files under `reviewed-mrs-automation/`. The historical !17571-!17620 batch remains part of the already-reviewed set. The fifty highest-numbered corpus MRs not already represented by that tracking were therefore !17708 through !17659 inclusive.

## Exact reviewed MR set

!17708, !17707, !17706, !17705, !17704, !17703, !17702, !17701, !17700, !17699,
!17698, !17697, !17696, !17695, !17694, !17693, !17692, !17691, !17690, !17689,
!17688, !17687, !17686, !17685, !17684, !17683, !17682, !17681, !17680, !17679,
!17678, !17677, !17676, !17675, !17674, !17673, !17672, !17671, !17670, !17669,
!17668, !17667, !17666, !17665, !17664, !17663, !17662, !17661, !17660, !17659.

Count: **50**.

## Review weighting and findings

Merged MRs were treated as stronger evidence than closed/unmerged work, and substantive maintainer discussion was weighted above mechanical approval/system notes. No Guy Harris review in this batch established a new convention requiring promotion to the notebook.

Representative durable or corroborating findings:

- **!17708 (merged, John Thacker):** DHCP option 43 handling tries known vendor-specific heuristic dissectors first, then falls back to the RFC-defined generic encapsulated suboption structure, leaving individual unknown suboption payloads opaque. This is a useful conservative-dissection example: decode structure that is known from the protocol grammar without pretending to know vendor-specific payload semantics. It corroborates existing guidance rather than requiring a new rule.
- **!17694 (merged, John Thacker):** SIMtrace response parsing slices the status word away before handing response data to a command-specific dissector and avoids forcing expected response bytes when a failed command supplied none. This reinforces the existing bounded-data and malformed-input guidance: child dissectors should receive a TVB representing only their semantic payload, and absence of optional/failed-operation data should not be turned into a synthetic malformed exception.
- **!17691 (merged, John Thacker):** `dftest` gains line-by-line filter input, including stdin, to amortize its expensive startup across large filter-validation sets. Useful testing/tooling ergonomics, but not a new coding convention.
- **!17684 (merged, John Thacker; Alexis La Goutte approval/backport discussion):** nonconformant syslog input preserves the remaining bytes as `syslog.msg` and marks the condition with expert info rather than silently discarding trailing data. The backport discussion explicitly notes restoration of the older useful dissection plus expert information. This reinforces the existing rule to preserve observable bytes while separately diagnosing malformed/nonconformant structure.
- **!17679 (merged):** fixes the JA4_c empty-extension-list case by assigning the defined empty value rather than processing an absent list as though populated. This is straightforward boundary-case handling and adds no new general convention.
- **!17673 (merged):** reduces memory consumption when hostname lookups are disabled by avoiding unnecessary name-cache population/copies. This is useful performance evidence but not sufficiently general or review-driven to add notebook guidance.
- **!17671 / !17663:** registration fixes/backport-style related work around Zigbee ZCL hf registration. These corroborate the already-recorded requirement that registered/used protocol fields and related tables be consistently initialized/registered.
- **!17670 / !17676 / !17677:** closely related USB generated-value normalization work; treated as related/superseding/backport-style evidence rather than independent architectural lessons.
- **!17667 (merged):** Qt Expert Info sorting changes group sorting to user-visible name ordering and protocol sorting to case-insensitive comparison. UI-specific and no broader convention extracted.
- **!17665 (merged):** makes IP/IPv6 conversation-ID assignment optional because the feature carries measurable CPU/memory cost. This is useful evidence that convenience metadata in hot dissector paths should justify its runtime cost, but the batch does not provide enough reviewer discussion to promote a new general rule.
- **!17660 (merged):** TCP desegmentation handles additional retransmission states. This reinforces the need for state-machine fixes to cover all relevant retransmission/desegmentation states, but is protocol-specific.
- **!17659 (merged):** removes obsolete AirPcap code; cleanup-only, no durable review convention.

The remaining MRs in the exact set were scanned for state, purpose, discussion signal, and changes. They did not contain stronger durable engineering guidance than the examples above or guidance already represented in the notebook. Closed/superseded or repetitive/backport work was deliberately given lower evidentiary weight.

## Notebook impact

No convention file was modified in this run. The durable observations above corroborate existing notebook guidance on conservative dissection, bounded TVBs/semantic payloads, preserving undecoded bytes with expert diagnostics, registration consistency, and defensive boundary handling. Adding duplicate rules would reduce the notebook's signal-to-noise ratio.

## Continuation

For the next backward review run, rebuild the reviewed set from all tracking rather than assuming a contiguous range. With the corpus and tracking state used here, the next apparent gap below this batch begins at **!17658**; note that **!17571-!17620 is already reviewed and must continue to count**, so selection must skip that historical batch when reached.