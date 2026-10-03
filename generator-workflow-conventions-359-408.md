# Generator workflow conventions from MRs 359-408

- Generation tools should operate end-to-end from their documented inputs rather than require hidden manual preprocessing; !397 was corrected by Pascal Quantin's !398 for exactly this reason.
- When a checked-in generated dissector artifact has a source or template change, regenerate and include the derived artifact in the same logical change (!366, Pascal Quantin review).
