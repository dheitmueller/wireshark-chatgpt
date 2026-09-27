# Wireshark Wiretap Timestamp Range Conventions

Merged master MRs !7798, !7794, !7795, !7796, !7792, and !7789, authored by Guy Harris, separate semantic timestamp storage from narrower capture-file encodings.

Internal timestamps should remain in the semantic host/Wireshark time type where possible. A writer must check that seconds fit the destination format's actual field width and signedness before conversion. If not, fail explicitly rather than truncating.

Different legacy formats can have different signedness even when each stores 32 bits. The validation belongs at the file-format boundary.

**Confidence:** Extremely high. Coordinated merged master fixes authored by Guy Harris.
