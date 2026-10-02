# Field and scripting conventions from MRs 1360-1409

Merged !1396 shows that a registered integer field must be wide enough for the byte length and mask used by its tree-add calls. Review field type, add-item length, mask, and stored value domain together.

Merged !1401 shows that generated information intended for Apply as Column or other downstream consumers belongs in the field value itself, not only in appended display text or hidden children.

Merged !1388 shows that a generated field can legitimately have no backing TVB. WSLua should represent that as an optional state, consistently with related properties, rather than treating semantic absence as an expired source object.
