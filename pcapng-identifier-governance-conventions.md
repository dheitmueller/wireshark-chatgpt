# pcapng Identifier-Governance Conventions

## Do not invent values in standardized block or option namespaces

Merged master MR !2424, authored by Guy Harris, adds explicit warnings beside pcapng block and option dispatch tables that every identifier handled as a standardized code must actually appear in the pcapng specification. A Wireshark-private feature must not claim an unassigned value in a standards-governed namespace merely because the parser can recognize it.

**Implementation rule:** use pcapng's private/custom extension mechanisms for private formats. If a new block or option is intended to be standardized, obtain the assignment through the standards process before adding it to the core standardized-code tables.

**Architecture rule:** keep namespace governance separate from extension dispatch. Generic plugin/extension mechanisms may handle assigned or private extension identifiers, but they do not authorize squatting on standardized code points.

**Confidence:** Extremely high. Merged master guidance authored by Guy Harris and consistent with the notebook's later pcapng extension-registration architecture.


## Proposed standard options must go through the pcapng registry; private options need a PEN

An implementation experiment does not authorize Wireshark to assign a new standardized pcapng option locally. If a feature needs a standards-space option, first take the proposal through the pcapng specification process. If it uses a custom option instead, encode the owner's IANA Private Enterprise Number as required by the format.

Open MR !1823 proposed a Section Header Block option for associating an external waveform with a capture. Guy Harris immediately asked whether the option had been submitted to the pcapng specification and said a standardized option would have to wait for acceptance there. When the author explained that the prototype used a custom option, Guy then verified the required PEN handling.

**Governance rule:** standards-space pcapng identifiers are assigned by the pcapng specification/registry process, not by the Wireshark implementation.

**Private-extension rule:** use the defined custom-option mechanism and validate/process the PEN rather than treating a private payload as though it occupied a globally standardized option code.

**Weighting note:** !1823 remained open in the reviewed corpus snapshot, so its implementation is not precedent. The durable evidence is the direct Guy Harris namespace-governance review, which independently corroborates the merged !2424 rule above.

**Confidence:** High for governance; lower for any implementation details of the still-open MR.
