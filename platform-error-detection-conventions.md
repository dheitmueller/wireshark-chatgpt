# Platform error detection conventions

User-facing capture diagnostics must follow the component actually in use, not just the operating system. Before recommending an Npcap bug report, for example, confirm that the runtime capture library is Npcap.

Do not recognize native failures by exact localized English error strings when a stable machine-readable discriminator exists. Windows library messages may embed arbitrary localized prose; match stable structure such as a known prefix plus native numeric error code where that is the reliable contract.

Diagnostics should preserve enough context to act on the problem: human-facing interface name, runtime capture-library identity/version, OS information when relevant, native error detail, and the correct upstream component for remediation.

Evidence: merged Guy Harris-authored !2013 through !2021 and !2028/!2029, with !2013 providing the localization-safe error-code pattern and !2029 the runtime-backend check.
