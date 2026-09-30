# Durable conventions from 3261-3310

## Logging
Merged 3297 makes wslog the shared diagnostic surface. Merged 3310 adds domain filtering and records that Critical and Error remain active independent of ordinary filters. Merged 3262 centralizes function-name provenance in ws_debug instead of duplicating it in caller strings. Merged 3293 keeps arguments syntactically referenced when debug/assert operations are compiled away so warning-clean builds do not gain unused-variable failures.

## Dependency discovery
Closed 3298 showed that changing one global GLib hint to match RHEL would break Windows vcpkg. Merged 3304 derives the Unix internal-header location from the GLib library actually selected while retaining the Windows package-layout rule. Companion headers should follow the chosen dependency artifact or platform package contract.

## Size domains
In closed 3268, Guy Harris explains that an in-memory buffer is bounded by size_t even if a wire or file length can be wider. Merged 3271 changes Export Object payload length to size_t. Wider lengths must be checked before narrowing into an address-space-sized value.

## Dissector boundaries and dispatch
Merged 3301 justifies a separate RDP dynamic-channel source module because it is a coherent subprotocol, is expected to grow, and is reused by another carrier. Closed 3289 contains high-authority Guy Harris guidance distinguishing fallback payload tables from field-keyed dissector tables and explaining why overlapping standard and extended CAN identifier namespaces need separate keyed tables. Prefer later merged CAN successors as implementation precedent.

## Generated code
Merged 3308 updates Kerberos ASN.1 conformance input and generated C together. Merged 3279 updates the Kerberos template and generated C together. Correct the authoritative generator/template input before regenerating derivatives.

## Submission and CI
Merged 3280 and 3279 show that valid author identity and Wireshark commit-message structure are submission requirements even when the source change is acceptable. In merged 3264 Guy Harris twice identifies unrelated edits; keep focused MRs free of drive-by changes. Merged 3276 shows that inherited CI setup is part of job behavior: a local before_script can replace rather than extend inherited setup.
