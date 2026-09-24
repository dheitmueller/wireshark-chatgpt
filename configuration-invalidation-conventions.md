# Configuration Invalidation Conventions

## Model independent invalidation domains explicitly

A configuration edit can invalidate one kind of derived state without invalidating every other kind. The flags on the configuration object should describe those effects independently rather than relying on an undocumented implication between them.

Merged master MR !10961, authored and merged by John Thacker, corrects the UAT behavior for display-filter macros. `UAT_AFFECTS_FIELDS` rebuilds the named-field environment, while `UAT_AFFECTS_DISSECTION` requests packet redissection; the former does not imply the latter. Display-filter macros need the field-environment refresh without packet redissection. Correcting the flag and its documentation fixes a crash when removing a macro invalidates the current display filter and a file is later opened or reloaded. Previously reviewed release counterparts !10978 and !10977 carry the same distinction.

**Implementation rule:** represent each invalidated cache or derived layer explicitly, and request only the effects the configuration change actually requires.

**Review rule:** verify configuration flags against the actual signal/refresh implementation. Do not infer that one flag implies another merely because most current users happen to set both.

**Confidence:** Very high. Merged master fix authored and merged by John Thacker with matching stable counterparts.
