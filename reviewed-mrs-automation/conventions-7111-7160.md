# Conventions from !7111–!7160

- !7141: Packet text converted to Wireshark's internal UTF-8 can change byte length. Validate protocol text before token parsing; do not use converted-string byte offsets as TVB offsets.
- !7134: Completed-reassembly lookup must carry enough packet context to reproduce the semantic reassembly key.
- !7156: Refining a wildcard UDP conversation can be wrong when exact-tuple lookup precedence and port reuse select an older conversation.
- !7151: Header-field masks define display-filter semantics; mask corrections can require release notes and baseline/test updates.
- !7128, !7132, !7138: When consumers need totals and a filtered subset, carry match state through one tap event stream and document callback API changes.
- !7131: Do not mechanically combine character encodings with `ENC_NA` when byte order is not meaningful.
- !7115: Before deleting GUI-looking command paths, trace shared CLI registrations, generated help/manpages, and tshark consumers.
- !7133: Represent mutually exclusive protocol modes with one enum/discriminant rather than independent booleans.
- !7119, !7120: Treat build-target names as interfaces consumed by CI, scripts, and documentation.
