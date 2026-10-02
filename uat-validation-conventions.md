# UAT validation and invalidation conventions

Validate UAT input at the configuration boundary before malformed values become active dissector state. Return an actionable error string that identifies the bad value/character rather than allowing invalid data to survive until dissection.

Treat invalidation dimensions separately. A UAT that changes ordinary dissection behavior needs `UAT_AFFECTS_DISSECTION`; if the table also determines dynamically registered fields, include `UAT_AFFECTS_FIELDS` so field metadata is rebuilt as well.

Prefer direct GLib allocation/formatting helpers for validation errors rather than redundant temporary allocation/copy code.

Evidence: merged !2054 (ESP key validation, with Gerald Combs review) and !2053 (SOME/IP dynamic-field invalidation).
