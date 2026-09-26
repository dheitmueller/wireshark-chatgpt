# Structured Error-Result Conventions

Merged MR !8948 changes display-filter compilation to return one structured diagnostic containing a code, message, and source location. John Thacker identified return paths where failure could be reported without defining that result; merged !8962 corrected the issue.

**Rule:** every recoverable failure path in an API that promises a structured diagnostic must leave that diagnostic in a defined state. Do not rely on a logging call being fatal unless the API contract guarantees that control cannot return.

**Confidence:** Very high; merged API change plus merged corrective follow-up.
