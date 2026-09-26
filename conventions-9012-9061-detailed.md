# Durable conventions from batch !9012-!9061

- Keep logical packet extent separate from snapshot byte availability. Presence decisions and safe byte reads use different length domains.
- In streaming decompression, track input consumption, output production, decoder status, and resource limits separately. A library return value should only be interpreted according to its documented contract.
- When a mature registration table gains a richer numeric key, use a namespace discriminator so new compound keys cannot collide with legacy keys.
- At file and protocol boundaries, use the encoding required by the external format instead of the host's local text encoding.
- A missing build-environment identity value can be an intentional validation signal. Defaults should not hide a mismatched or uninitialized toolchain environment.
- Format-specific string rules take precedence over generic text-repair behavior; termination and padding semantics are part of the format contract.
- Establish a nested item's semantic boundary before parsing its contents, and keep displayed source ranges tied to that bounded item.
- New or substantially changed dissectors benefit from focused representative captures, fuzzing, static-analysis checks, and validation of field-registration uniqueness.
