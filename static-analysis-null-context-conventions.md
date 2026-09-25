# Static-Analysis Nullable-Context Notes

Wireshark merge request 9372 added HTTP/2 window tracking. During review, Alexis La Goutte reported a Clang Analyzer null-dereference path involving the HTTP/2 session context. After merge, John Thacker reported that the change caused issue 18815. The contributor then confirmed that an existing helper legitimately calls the affected path without a session object and opened corrective merge request 9534.

Durable convention: a static-analysis warning on a newly dereferenced context is checked against every supported entry path before merge. If an existing caller legitimately omits that context, new state tracking preserves the nullable contract or deliberately changes all callers together. A warning that maps to a real nullable call path is treated as likely correctness evidence rather than analyzer noise.

This is negative evidence from a merged change that regressed and was corrected; it is not an implementation exemplar.

Confidence: very high.
