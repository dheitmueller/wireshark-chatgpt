# Conditional subsystem initialization conventions

Expensive optional subsystems, especially those that spawn helper processes, should be initialized only when the requested operation needs them. Do not make every CLI startup pay the discovery/registration cost merely because the subsystem exists.

When initialization is intentionally skipped, configuration parsing must distinguish "known subsystem not registered in this execution path" from a genuinely unknown preference. That compatibility behavior should be narrow and explicit.

Measure the effect. In merged !2035 Gerald Combs compared both wall-clock test time and process creation count on Windows; unconditional extcap preference registration caused a large regression, while demand-driven registration recovered much of the cost.

Evidence: merged !2035.
