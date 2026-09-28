# Durable conventions from !5811–!5860

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- !5840 with !5847/!5848: nested parsing is bounded by the enclosing declared semantic container, not by all remaining packet bytes.
- !5853, with Guy Harris review: pcapng link-layer/interface metadata can be discovered after open; streaming consumers must not assume IDBs are front-loaded or that output metadata is fully known before records arrive.
- !5829 and !5835, both by Guy Harris: exception catcher stacks are thread-local execution state; worker-thread registration failures are caught, converted to owned diagnostic data, joined, and rethrown on the controlling initialization thread.
- !5856 by Gerald Combs: dependency discovery includes the version, and an incompatible new major API is rejected until Wireshark is deliberately ported.
- !5857 by John Thacker: byte-preserving round-trip tests use original frame bytes, not decrypted/reassembled secondary data sources.
- !5827: packet-controlled cryptographic size limits should be visible through expert diagnostics so a future legitimate larger value is distinguishable from silent parser failure.
- !5824 and !5839 by John Thacker: final semantic validation belongs in the shared import engine; GUI enable/disable state and caller-side checks are not sufficient by themselves.
- !5816: intermediate commits can aid review, but Pascal Quantin notes that Wireshark utility rebases can defeat GitLab automatic squash; requested final squashing should therefore be explicit before merge.

Closed !5859/!5852 and open !5815 were retained only as workflow/rejected-design evidence.
