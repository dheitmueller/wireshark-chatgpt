# Conventions from MRs 10463-10512

- MRs 10510 and 10511: validate a field's required byte span before lower-level access; use normal parse errors for invalid input and assertions for internal invariants.
- MRs 10488 and 10505: release model-owned data at the model's lifetime boundary rather than when the capture source closes.
- MR 10493: static-checker cleanup still requires semantic review across all uses; a clean checker result is not proof of behavior.
- MR 10471: change authoritative ASN.1/template input and regenerate checked-in output.
- MR 10474 with 10489 and 10490: keep fixes separable from enhancements so supported release branches can take the fix.
