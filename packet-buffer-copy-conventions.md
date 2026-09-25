# Packet Buffer Copy Notes

Wireshark merge request 9376, authored and merged by Gerald Combs, changed E2AP and LoRaWAN so packet-buffer bytes are transferred with the packet-buffer library's copy helper instead of first requesting a direct memory view. Gerald noted that the older form was valid but preferred minimizing direct packet-buffer views. Martin Mathieson and Ales Povalac checked the affected protocols, and review found another LoRaWAN occurrence. Merge request 9374 independently made the same style change in ALP.

Durable convention: when code only needs to transfer bytes from a packet buffer into caller-owned storage, it uses the packet-buffer transfer helper directly. Direct memory views are kept for cases that actually need a view rather than as an unnecessary intermediate representation.

Confidence: very high.
