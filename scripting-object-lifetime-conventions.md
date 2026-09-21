# Wireshark Scripting Object Lifetime Conventions

This file records durable conventions for object lifetime at Wireshark scripting-language boundaries. Current upstream source remains authoritative.

## Keep a scripting-language root for script-owned objects retained by native wrappers

A native wrapper that stores a raw pointer derived from a Lua object does not, by itself, keep the Lua object alive. If the wrapper must later expose the original object or relies on storage owned by that object, it must retain an explicit Lua reference for the same lifetime as the native wrapper and release that reference during wrapper destruction.

Merged master MR !15144 adds read-only `ProtoField` attributes to WSLua. For value-string/unit objects, the native `wslua_field_t` already retains a C-side `vs` pointer, but the accepted implementation also stores the original Lua value in the registry with `luaL_ref()`. The getter returns that registry-held value with `lua_rawgeti()`, and `ProtoField__gc` releases it with `luaL_unref()`. Fields with no such script-owned object use `LUA_NOREF`.

**Implementation rule:** when a C wrapper retains or re-exposes state originating in Lua, identify which runtime owns the backing object. If Lua owns it, pair the native reference with a registry reference (or equivalent scripting-runtime root) and release both at the same lifecycle boundary. Do not assume a converted/raw C pointer is a garbage-collector root.

**Review rule:** for newly exposed WSLua attributes and callbacks, trace both the native object's lifetime and the Lua object's lifetime. Verify construction, getter/callback use, error paths, and `__gc`; stale pointers can remain latent until a later attribute access even when initial construction succeeds.

**Confidence:** High. The evidence is an accepted merged master WSLua change approved and merged by Anders Broman, with explicit registry-reference acquisition and release in the final implementation.
