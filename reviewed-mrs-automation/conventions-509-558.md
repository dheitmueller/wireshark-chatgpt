# Conventions 509-558

- Reassembly keys include all stream identity fields (!539).
- Weak heuristics default disabled (!522).
- Return parser status separately from numeric data (!509/!510).
- Use add-and-return field APIs when the same field is displayed and consumed (!514).
- Prefer semantic public types such as `ws_in4_addr` and update export metadata (!556).
- Shared hidden fields can provide cross-protocol filters without replacing protocol fields (!530).
- Use explicit lookup failure rather than fallback-string comparison (!552).
- Check packet protocol-data pointers before use (!557).
- Generated translations and image assets follow repository tooling (!524).
