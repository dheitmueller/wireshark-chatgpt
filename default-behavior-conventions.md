# Wireshark Default-Behavior Conventions

This file records durable review guidance for choosing defaults when Wireshark exposes multiple legitimate interpretations or modes. Current upstream source remains authoritative.

## Choose defaults for the semantics of the selected mode, not a narrower secondary use case

A supported preference can legitimately serve several workflows, but changing its default should be justified by what users are normally asking the selected mode to do. A niche workflow that benefits from the alternate setting is not by itself evidence that every user of that mode should get the alternate behavior, especially when the existing preference already makes the niche behavior available.

Closed/unmerged MR !13090 proposed enabling “Dissect next layer” by default for the file-pcap, file-pcapng, and file-btsnoop dissectors. Pascal Quantin objected that opening a capture *as a file format* normally indicates interest in the file-format structure itself and that automatically dissecting all contained traffic clutters the Info column because the file is represented as one packet. The submitter's motivating case was a pcapng containing GRE inside a custom protocol. Guy Harris then gave the higher-level framing: either default will fail some users, and if the goal is ordinary packet dissection, the relevant question is why the capture is being dissected as a file rather than simply opened as a capture. Anders Broman ultimately closed the MR because the use case was debatable and the requested behavior remained available through the existing preference.

**Review rule:** for a default change, identify the semantic intent of the mode in which the option appears and test the proposal against common workflows for that mode. Do not infer a new global default from a specialized workflow that can already opt in.

**UX rule:** when two behaviors are both legitimate and neither is universally correct, keeping the established default plus an explicit preference can be better than flipping the default. A default change needs evidence that the balance of expected behavior has changed, not merely evidence that the alternative is useful.

**Confidence:** Medium-high. The implementation itself was not merged, so it is weaker evidence than accepted code. However, the rationale was explicitly discussed by Pascal Quantin and Guy Harris and the MR was closed by Anders Broman on that basis, making it strong review-policy evidence rather than implementation precedent.