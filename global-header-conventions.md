# Wireshark Global-Header Conventions

This file records durable conventions for Wireshark's project-wide umbrella header. Current upstream source and developer documentation remain authoritative.

## Keep the global header intentionally narrow and layering-safe

Merged master MR !4645 introduces `wireshark.h` together with explicit architectural documentation and substantial review. The accepted design is not a general-purpose “include everything” header. Its constraints are the important part:

- keep it small because every change has wide rebuild and error-surface impact;
- do not make it depend on build-machine configuration;
- only depend on Wireshark headers that are installed for consumers/plugins;
- only place definitions there that are valid globally, without creating upward library/component dependencies.

Graham Bloice explicitly raised the “one big header” concern. João Valverde agreed that avoiding bloat is essential and limited the intended contents to mechanisms broadly required across Wireshark. Guy Harris then asked whether C files had any reason to include `ws_assert.h`, `wslog.h`, and `wmem.h` directly once those facilities were intentionally supplied by `wireshark.h`; João answered no.

**Architecture rule:** use an umbrella header only as a deliberately small project baseline. Add something because the facility is genuinely global and layering-safe, not merely to avoid an explicit dependency.

**Plugin/public-header rule:** anything reachable through the global header must exist in the installed consumer environment. Do not leak private generated headers or non-installed internal dependencies into that include graph.

**Include rule:** once a facility is intentionally part of the global baseline, individual source files do not need redundant direct includes solely to obtain it. This does not remove the obligation to include the specific module/API headers that define the interfaces a translation unit uses.

**Confidence:** Very high. Merged master architecture/documentation change with direct Guy Harris review.
