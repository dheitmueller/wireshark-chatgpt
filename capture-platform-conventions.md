# Capture Platform Conventions

This file records durable platform-specific capture initialization and compatibility guidance.

## Request UTF-8 libpcap diagnostics on Windows when the runtime supports it

Windows capture code should avoid inheriting an arbitrary local-code-page contract for libpcap/Npcap diagnostic strings when the library offers an explicit UTF-8 mode.

Merged MR !23020, authored by John Thacker and approved by Anders Broman, uses `pcap_init()` on Windows when that API is available and requests UTF-8 error strings instead of the current Windows code page. This also disables legacy `pcap_lookupdev()` device-string encoding behavior that is unnecessary for current Wireshark usage. The API was introduced in libpcap 1.10.0, so the change preserves runtime compatibility rather than assuming every supported installation exports it.

The motivation is reinforced by Guy Harris's review of related draft MR !23013. Guy explained that `pcap_init()` and `PCAP_CHAR_ENC_UTF_8` were designed in part to let applications such as Wireshark opt out of legacy code-page diagnostics without breaking older WinPcap/Npcap applications that might depend on them. He also explicitly noted that runtime use requires an availability wrapper rather than unconditional linkage.

**Platform rule:** where optional libpcap APIs allow Wireshark to request UTF-8 on Windows, prefer that explicit encoding contract and gate it by actual runtime/API availability. Do not make downstream UI/error handling guess or translate an unspecified local code page when the capture library can supply UTF-8 directly.

**Confidence:** Very high for the Windows rule: merged master implementation plus direct high-authority rationale from Guy Harris. The separate Linux timeout proposal in !23013 remained draft/open and is not promoted here.