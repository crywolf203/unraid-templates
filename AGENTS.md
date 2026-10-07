# Project maintenance instructions

When adding or changing user-facing AMA configuration fields, update the AMA template in this repository as part of the same feature work, together with the runtime defaults and documentation in crywolf203/ama-unraid.

- Expose new user-facing settings in Unraid with clear names, defaults, descriptions, and dependencies.
- Keep related settings together in display order. Put a feature's enable switch first, followed by the settings needed to configure it.
- Keep Plex notification settings visible together: Notify Plex, Plex Library Name, Plex URL, Plex Token, then the optional Plex Scan Path Override.
- Match template defaults to the runtime/image defaults. Do not silently enable opt-in features.
- Preserve masking for credentials and existing user values.
- Validate XML and ensure there are no duplicate configuration targets.
- Describe template migration for existing installations; an image update alone must not be assumed to add fields to the user's saved template.

