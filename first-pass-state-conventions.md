# First-Pass State Conventions

Merged MR !8917 includes Pascal Quantin review explaining that the initial dissection pass is ordered by frame number while later redissection can occur in arbitrary order.

**Rule:** state changes that are valid only while building sequential capture history should be explicitly guarded at the state-change site. Do not rely on distant control flow to make later redissection harmless.

**Confidence:** Very high; merged fix with direct maintainer review.
