# Review findings: Wireshark MRs !6161–!6210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is weighted more heavily than closed or superseded work. Direct maintainer guidance is called out where it establishes a durable convention.

| MR | Outcome | Finding |
|---|---|---|
| !6210 | Merged | Adds MPEG Short Smoothing Buffer descriptor decoding with explicit masks and value tables. |
| !6209 | Merged | Adds MPEG Time Shifted Event descriptor decoding with explicit service/event identifiers. |
| !6208 | Merged | Adds XAIT Content Location descriptor support and cites ETSI TS 102 727 beside the registration. |
| !6207 | Merged | Adds Service Identifier descriptor support from ETSI TS 102 812. |
| !6206 | Merged | Replaces deprecated Windows QProcess single-command invocation with program + argument list. Roland Knall explicitly raised path-with-spaces behavior; the revised form was tested on Windows with and without spaces. |
| !6205 | Merged | Gerald Combs moves costly Ubuntu package production post-merge while retaining a package test, and moves latest-Clang coverage into MR pipelines because it catches defects relevant to macOS. |
| !6204 | Merged | Uses the registrar macro that already includes the required assertion rather than repeating lookup + assertion boilerplate. |
| !6203 | Merged | Updates Profinet enumerations and reserved ranges; Uli Heilmeier explicitly required the neighboring comment to be updated too. |
| !6202 | Merged, later corrected | Forces a Qt selection change to refresh packet details, but a later report showed it broke packet-history navigation and John Thacker pointed to !16714. Treat synthetic selection mutation as cautionary evidence. |
| !6201 | Merged | Release-3.4 version bump. |
| !6200 | Merged | Release-3.6 version bump and release-note reset. |
| !6199 | Merged | Moves Debian packaging sources under `packaging/debian` and updates tools, docs, CI, and release helpers. Gerald retains a compatibility symlink because Debian tooling expects `debian/`. |
| !6198 | Merged | Builds the 3.4.12 release. |
| !6197 | Merged | Builds the 3.6.2 release. |
| !6196 | Merged | Accepted Geneve option-class registry update and clean successor to !6195. |
| !6195 | Closed / superseded | Uli Heilmeier asks for a component-prefixed commit subject and a feature/topic branch rather than the contributor's master branch. The contributor opens !6196, which merges. |
| !6194 | Merged | Adds newly supported protocols to release notes. |
| !6193 | Merged | Disables release-3.4 documentation publication until versioned docs exist because the job would otherwise overwrite master documentation. |
| !6192 | Merged | Same documentation-publication guard on release-3.6, reinforcing !6193. |
| !6191 | Merged | Promotes `dissect_bluetooth_common()` to public API and updates both DLL export declaration and Debian symbol manifest. |
| !6190 | Merged | Adds Fortinet 802.11 vendor-specific decoding and a centralized OUI definition. |
| !6189 | Merged | Stable backport of !6188; resets a reused `wtap_rec` on the non-match path before its owned block pointer is lost. |
| !6188 | Merged | John Thacker fixes a Find Packet leak by resetting the current Wiretap record before reuse after an unsuccessful match. |
| !6187 | Merged | Release-3.4 preparation. |
| !6186 | Merged | Release-3.6 preparation; discussion records accepted inclusion of the focused !6188 leak fix via !6189. |
| !6185 | Merged | Adds missing registration/handoff prototypes to satisfy `-Wmissing-prototypes`. |
| !6184 | Merged | Optimizes Find Packet and fixes wide-string matching. Gerald's 32-bit Windows failure exposed pointer-difference type trouble; John called eliminating the subtraction better practice, later fixed by !6238. |
| !6183 | Closed / superseded | Proposed a broad HTTP/2 recovery revert. John Thacker identified a narrower nghttp2-aware solution and closed this in favor of merged !6573. |
| !6182 | Merged | Cleans field masks and checker behavior; useful typed-field hygiene, mostly mechanical. |
| !6181 | Merged | Guy Harris makes field-definition validation explicitly type-aware, removes fallthrough that admitted nonsensical display metadata, and improves diagnostics to name the allowed display-info category. |
| !6180 | Merged | Documentation icon update. Gerald Combs explicitly evaluates asset-source consistency and license compatibility before settling on a coherent icon family. |
| !6179 | Merged | Release-3.4 backport: FT_UINT_BYTES/FT_UINT_STRING item length must be at least the count-field width, not merely nonnegative. |
| !6178 | Merged | Release-3.6 counterpart of !6179. |
| !6177 | Merged | Release-3.4 backport of BP malformed-length progress hardening. Error paths return a value that prevents caller re-entry at the same position. |
| !6176 | Merged | Release-3.6 counterpart of !6177. |
| !6175 | Merged | GDSDB stable fix checks minimum length, wrap of derived next offset, and monotonic advancement after opcode handlers; malformed/no-progress cases emit expert info and terminate safely. |
| !6174 | Merged | Release-3.6 counterpart of !6175. |
| !6173 | Merged | John Thacker fixes TCP sequence accounting so SYN/FIN's one sequence-space unit is included when comparing expected segment end against `nextseq`. |
| !6172 | Merged | Stable p_mul fix represents a missing-sequence interval with from/to fields instead of materializing every missing sequence as a generated field. |
| !6171 | Merged | Release-3.6 counterpart of !6172. |
| !6170 | Merged | Qt layout correction prompted by platform-dependent QDialogButton ordering. |
| !6169 | Merged | WAP stable hardening clamps decoded variable-length quantities before downstream arithmetic; wrap detection alone is not enough for huge but representable values. |
| !6168 | Merged | Release-3.6 counterpart of !6169. |
| !6167 | Merged | RTMPT stable hardening imposes a finite AMF iteration cap and expert diagnostic to stop crafted excessive/infinite loops. |
| !6166 | Merged | Release-3.6 counterpart of !6167. |
| !6165 | Merged | Guy Harris stable fix corrects a reversed zero-progress predicate in ZigBee ZCL; valid nonempty elements had been rejected by the earlier defensive fix. |
| !6164 | Merged | Release-3.6 counterpart of !6165. |
| !6163 | Merged | Master BP fix ensures malformed SDNV/length paths return progress-safe values and adds checks around derived block lengths. |
| !6162 | Merged | Guy Harris master fix: the no-progress condition is `new_offset <= old_offset`; the prior inverted comparison broke ordinary positive-progress items. |
| !6161 | Merged | CMS security backport replaces packet-derived globals with packet-scoped proto data and explicitly clears OID/content slots before each semantic decode, preventing cross-packet and same-packet stale state. Generator source and generated dissector are updated together. |
