# Qt Dialog Persistence Notes

Wireshark merge requests 9394, 9395, and 9396 changed Coloring Rules so persistent color data is stored before the dialog completes successfully. This prevents packet-list recoloring from starting against data that has not yet been stored. The master change was authored by John Thacker; the release-branch copies were merged by Guy Harris.

Durable convention: operations initiated by an OK or Save action are ordered so that persistent state is established before a successful dialog transition becomes visible to dependent UI code. Storage failure does not publish a successful transition.

Confidence: extremely high.
