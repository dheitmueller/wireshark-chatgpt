# Durable conventions from Wireshark MRs 5561-5610

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## Keep option presence separate from the value domain

Guy Harris's merged master !5566 replaces an out-of-range sentinel used to mean "-i was not supplied" with an explicit Boolean presence flag. His merged !5567 then parses the option into its real guint8 domain with ws_strtou8 and funnels all ways of selecting the IP next-protocol value through a common setter.

Use a separate presence bit/state for an optional value instead of reserving an impossible numeric value, especially when the value is or becomes unsigned. Parse directly into the semantic width when a checked Wireshark helper exists, and centralize the secondary state changes caused by the option. Closed !5563 and !5562 are useful history but are not accepted precedent.

Confidence: extremely high; both authoritative changes are merged master MRs authored by Guy Harris.

## Validate a wide conversion before narrowing it

Merged !5565 accidentally cast strtoul's result to guint32 before storing it in an unsigned-long temporary, making its later G_MAXUINT32 comparison unable to detect truncation. Gerald Combs's merged !5570 removes that cast specifically because the range is checked afterward.

Keep the conversion result in a type that can represent the parser's full result, perform errno/end-pointer/range checks there, and narrow only after validation. Do not use a cast to silence a narrowing warning before the check that proves narrowing is safe. Guy Harris's !5567 shows the preferred alternative when a width-specific project helper already expresses the destination range.

Confidence: very high; the later merged correction explicitly repairs the earlier merged mistake.

## Guard line-scanning loops at the source boundary

Guy Harris's merged master !5573 explains that tvb_find_line_end can return a zero-length line without advancing next_offset when called past the tvbuff end with reassembly disabled. Replacing unbounded loops with while (tvb_offset_exists(tvb, offset)) makes the source boundary part of the loop invariant. Stable !5574 and !5575 carry the same correction.

If a loop's meaning is "while input remains", encode that condition directly rather than relying on a helper to manufacture progress or a distinctive sentinel after the cursor is already out of range. Test exact-end, empty/truncated input, and normal positive-progress lines.

Confidence: extremely high; master and both maintained branches accepted the Guy Harris-authored fix.

## Match configure probes to the API form being detected

Merged !5594 fixes timespec_get and related detection by using CMake check_symbol_exists with the declaring header instead of check_function_exists. The MR records why: link-only function probes miss inline functions and macros, can fail on 32-bit Win32 calling conventions, and do not prove that the expected declaration is available from the headers used by source code.

Choose a configure test that answers the source-level question the caller actually depends on. Declaration-aware symbol probing is appropriate for header-provided APIs; later notebook guidance still applies that build-host declaration/linkability is not by itself proof of deployment-target runtime availability.

Merged !5597 complements this by hiding strptime feature-test macros and system-versus-fallback selection behind ws_strptime rather than repeating platform exposure rules at each caller.

Confidence: high; both are merged master portability changes.

## Model mutually exclusive semantic choices explicitly in the UI

During merged !5600, Guy Harris points out that an "IPv6" checkbox makes the unchecked IPv4 meaning implicit and recommends an explicit IPv4/IPv6 choice. He also identifies the ambiguity of inferring a family when no addresses are supplied and defaults must be generated. John Thacker's merged follow-up !5632, reviewed in the preceding batch, implements the clearer combo-box model and stores the semantic IP-version value rather than treating widget state as the external identity.

When two alternatives are peer semantic modes, present and persist the mode itself. Avoid a checkbox whose false state silently means another named mode, and do not infer a semantic mode from optional fields when those fields may legitimately be absent. GUI enablement and persistence should derive from that semantic state.

Confidence: extremely high; direct Guy Harris design review followed by an accepted John Thacker implementation.

## Corroborating conventions already represented elsewhere

!5582 independently reinforces use of pinfo->pool for packet-lifetime temporary storage, removal of manual frees after adopting scope-managed memory, and early validity checks before allocation. Those rules are already captured in allocator-scope-conventions.md.

Closed !5606 is valuable review history because João Valverde and John Thacker used the discussion to uncover the ISO-8601 offset-sign defect, but the draft itself is not precedent. The authoritative implementation and testing rule are merged !5668/!5669 and are already recorded in time-parsing-conventions.md.

!5609 provides useful type-domain evidence: if a metadata field already carries display categories, avoid a second overlapping enum that requires casts; use the common enum and validate the narrower subset at APIs that require one category. The accepted code removes the parallel absolute-time enum and adds explicit FIELD_DISPLAY_IS_ABSOLUTE_TIME checks.
