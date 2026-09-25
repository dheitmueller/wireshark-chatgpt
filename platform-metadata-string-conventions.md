# Platform Metadata String Notes

Wireshark merge request 9392, authored and merged by João Valverde, trims cosmetic edge whitespace from the CPU model name before adding it to version information. Guy Harris asked which systems showed the issue and noted that processor brand strings can legitimately contain leading blanks; Windows may preserve those blanks while another operating system may normalize them.

Durable convention: host metadata intended only for human-readable presentation is normalized at the presentation boundary when leading or trailing padding has no semantic meaning. Code does not assume different operating systems perform identical cosmetic normalization.

Confidence: extremely high.
