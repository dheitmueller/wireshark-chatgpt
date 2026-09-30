# Dissector Module Boundary Conventions

Merged master MR 3301 adds RDP dynamic virtual channel support. Alexis La Goutte questioned whether another source file was necessary. The contributor explained that the existing RDP dissector was already large, the dynamic-channel decoder would continue to grow, and the same dynamic-channel protocol is reused from another carrier path. Alexis accepted the split.

Rule: avoid creating source files merely to reduce line count. A separate module is justified when it owns a coherent subprotocol, is expected to evolve independently, and can be reused from multiple carrier or transport dissectors.

Review rule: when proposing a split, explain semantic ownership and reuse. That is stronger evidence than size alone.

Confidence: high. Direct maintainer question and acceptance on a merged master feature.
