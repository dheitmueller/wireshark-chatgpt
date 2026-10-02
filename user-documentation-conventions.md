# User Documentation Conventions

## Explain what a UI or statistics surface shows and how the reader can use it

User documentation should do more than state that a statistics window or dialog exists. Describe the information exposed, where important values come from, and the practical interpretation needed for a reader to understand why the surface is useful.

Merged documentation MRs !1803, !1801, and !1775 repeatedly received this review direction. Moshe Kaplan rejected prose that amounted to "statistics are available" and asked what the statistics mean and when they are useful. Peter Wu asked for concrete dimensions and data provenance, recommended reader-directed language ("you"), and requested project-internal cross-references for configuration material so offline documentation remains useful. Anders Broman similarly replaced vague DHCP wording with the concrete fact that the table counts occurrences by DHCP message type.

**Content rule:** document observable fields and useful interpretation, not merely the existence or menu location of the feature.

**Voice rule:** where the surrounding User's Guide addresses the reader directly, use reader-facing wording rather than repeatedly referring to "the user."

**Linking rule:** for project documentation that must work in generated or offline forms, prefer internal anchors and cross-references over links back to the live web copy of the same manual.

**Confidence:** Very high. Three merged documentation MRs with repeated review from Peter Wu, Moshe Kaplan, and Anders Broman.
