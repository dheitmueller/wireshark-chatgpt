# Wireshark Release-Notes Submission Conventions

This file records durable release-note expectations extracted from upstream maintainer review. Current upstream contribution and release guidance remains authoritative.

## Document user-visible bundled dependency changes

A dependency-version change is not necessarily invisible implementation maintenance. When Wireshark installers ship a different bundled runtime/capture dependency that users may care about operationally, update the release notes together with the packaging change.

Merged MR !17589 (`NPCAP: Update to 1.80`) updates the bundled Windows Npcap package. Gerald Combs explicitly asked, “Can you update the release notes as well?” Merged follow-up !17603 (`Mention NPCAP update.`) adds the user-facing note that Windows installers now ship with Npcap 1.80 instead of 1.79.

**Submission rule:** when changing a bundled dependency in a way visible or relevant to users—especially installer/runtime components—check whether the release notes need a corresponding entry and include it in the same submission when practical. Treat release-note integration as part of feature/package completeness rather than post-merge housekeeping.

**Confidence:** Very high. Direct review request from project leader Gerald Combs followed immediately by the merged release-note change.
