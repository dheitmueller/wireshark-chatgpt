# Conversation Endpoint Override Notes

Wireshark merge request 9373, authored by Guy Harris, removed an EAP path that copied an entire packet_info structure and then edited the copy's addresses and ports for conversation lookup. The accepted change uses the conversation API that explicitly supplies alternate endpoint addresses and ports.

Durable convention: packet dissection context and conversation-key construction remain separate abstractions. When conversation identity differs from the packet's literal endpoints, the dedicated conversation endpoint/key API represents that difference; packet_info is not cloned and edited as a surrogate conversation key.

Confidence: extremely high.
