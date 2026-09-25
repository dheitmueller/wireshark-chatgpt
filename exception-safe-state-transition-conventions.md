# Exception-Safe State Transition Conventions

## Do not bracket exception-capable dissection with temporary persistent-state mutation

Merged master MR !10536, authored and merged by John Thacker, removes MySQL's temporary `MYSQL_COMPRESS_PAYLOAD` conversation state. In closed precursor !10531, John identified the failure mode directly: `dissect_mysql_pdu()` can throw, so changing persistent state before the call and restoring it afterward can leave the restoration unreachable.

The accepted design makes compression state monotonic and selects the correct PDU dissector before entering `tcp_dissect_pdus()`.

**Implementation rule:** persistent dissector state must not depend on a set-call-restore sequence that spans code capable of throwing. Prefer monotonic state, explicit call context, or dispatch chosen before the exception boundary. If temporary mutation is unavoidable, restoration must be guaranteed on exceptional exit as well as normal return.

**Confidence:** Very high. The exception-safety rationale is explicit in John Thacker's review and the accepted successor implements that direction.
