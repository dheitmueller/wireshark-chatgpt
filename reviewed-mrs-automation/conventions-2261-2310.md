# Durable convention synthesis — !2261–!2310

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- !2310 and !2276 (Guy Harris): capture channel metadata can differ from the actual per-packet 802.11 modulation. Prefer explicit per-packet modulation metadata and data rate; use frequency only to resolve remaining ambiguity.
- !2268 and !2284: runtime assertions may be absent in some builds, so they are not a substitute for packet validation or memory-safety checks. Test assertions intended to validate the test must remain active.
- !2264 (João Valverde review): keep regression captures small and avoid making large blocks of ordinary tshark display text into a stable test interface. Test stable semantic output instead.
- !2295: expert-info group and severity are separate domains and should be validated independently.
- !2281: keep concise error diagnosis separate from longer remediation text so repeated interface failures do not flood logs.
- !2280: preserve explicit log-domain configuration and use domain-aware logging.
- !2270 (Anders Broman review): do not repeatedly rebase a live MR merely because the target branch advanced; unnecessary rebases consume CI and review attention.
- !2301 (John Thacker): stop child PSI reassembly at the section's declared boundary and treat remaining MPEG-TS payload bytes as stuffing.
- !2288: values reconstructed from request state with no backing bytes in the current packet are generated fields.
- !2291 with Guy Harris review: prefer `wmem_realloc()` for same-scope contiguous growth instead of manual copy-size bookkeeping; !2724 later implements that direction.
