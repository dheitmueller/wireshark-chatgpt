# Cross-Frontend Registration Conventions

This file records durable conventions for code and plugins shared by Wireshark, Stratoshark, and other frontends that initialize overlapping but non-identical protocol infrastructure.

## Registration code must tolerate tables or capabilities absent from the current frontend

Shared dissector/plugin registration paths cannot assume that every frontend has created the same dissector tables or enabled the same table capabilities. Before registering a range preference or Decode-As handler, resolve the target table and verify the operation it supports. If a table is legitimately absent in the current application, skip the registration with a useful diagnostic rather than dereferencing missing state or turning application startup into a dissector assertion/crash.

Merged MR !23226, authored and merged by Gerald Combs, was driven by Stratoshark startup on Windows when Wireshark and Stratoshark plugins shared the same build directory. Wireshark-oriented registrations referred to tables such as `udp.port` and `ethertype` that were not present in that Stratoshark initialization context. The accepted change makes `dissector_add_range_preference()` check for a valid table and also conditions Decode-As registration on the table's `supports_decode_as` capability. Gerald's testing showed the change converting an immediate crash into diagnostics that exposed the remaining incompatible registrations.

**Implementation rule:** treat a dissector table as a runtime capability of the current registration environment, not a process-global certainty implied by a plugin having compiled successfully. Shared registration helpers should validate table existence and table capabilities at their API boundary.

**Confidence:** Very high. Merged master change authored and merged by Gerald Combs, with direct cross-frontend failure reproduction and validation.