# Durable conventions from Wireshark MRs !7911-!7960

- **Conversation terminology and side effects (!7931, !7934, Guy Harris):** a conversation is not an endpoint. Conversation-element setters both construct identity and install it into `packet_info`, so names should expose that state-changing behavior.
- **Type domains (!7915, Guy Harris):** independent enum domains are not interchangeable merely because their numeric constants currently match. Store and pass the semantic type the API expects.
- **TCP reassembly viewpoint (!7949, John Thacker):** sender-side retransmission classification and reassembler “already seen” state answer different questions. Reassembly should use its own state when delivery guarantees permit it.
- **Test prerequisites (!7926):** build-system test entry points should declare helper-program prerequisites explicitly. CTest fixtures can build `test-programs` before the unit suite; direct pytest is a separate workflow.
- **Transport-scoped sequence state (!7922):** persistent sequence history belongs at the protocol-defined transport/session scope, while packet-specific conclusions belong to packet proto-data.
- **Generated source of truth (!7948):** edit authoritative generator/input data such as `.mailmap`, not the generated artifact a reviewer happened to notice.
- **Analyzer visibility (!7940, !7960):** protocol analyzers can report anomalies beyond endpoint parsing behavior and can continue bounded parsing of compatible unknown versions when doing so preserves useful visibility without pretending full support.
