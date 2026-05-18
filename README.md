<p align="center">
 <img src="https://raw.githubusercontent.com/crywolf203/unraid-templates/main/unraid-templates-icon.png" alt="Unraid Templates by crywolf203" width="160">
</p>

<p align="center">
 <strong>Personal Unraid Community Applications Templates for apps I actually use on my own Unraid server.</strong>
</p>

<p align="center">
 <a href="https://unraid.net">
 <img alt="Unraid" src="https://img.shields.io/badge/Unraid-Templates-f15a24?style=for-the-badge">
 </a>
 <a href="https://github.com/crywolf203/unraid-templates/blob/main/LICENSE">
 <img alt="License" src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge">
 </a>
</p>

---

## Overview

This repository contains Unraid Community Applications XML templates maintained by [`crywolf203`](https://github.com/crywolf203).

The goal of this repo is simple:

> Create clean, useful Unraid templates for applications I personally run, test, and use on my own Unraid server.

These templates are intended to make installation easier for other Unraid users while keeping the setup close to the way the app actually works in Docker.

---

## Important note about these templates

The templates in this repository are generally **wrappers** around other applications.

That means:

- I do not claim ownership of the upstream apps.
- I do not rebrand the upstream apps as my own.
- I only package or template them for easier Unraid use.
- App-specific bugs should usually be reported to the upstream project.
- Docker, Unraid template, path, permission, and container issues can be reported here or in the related container repository.

Whenever possible, each template links back to the original upstream project.

---

## Current templates

| App | Description | Template | Upstream Project | Container |
|---|---|---|---|---|
| LRCGET | Browser-accessible Unraid wrapper for LRCGET, a tool for downloading synced `.lrc` lyrics for offline music libraries. Includes WebUI audio support with `WEB_AUDIO=1`. | [`templates/lrcget.xml`](templates/lrcget.xml) | [`tranxuanthang/lrcget`](https://github.com/tranxuanthang/lrcget) | [`crywolf203/lrcget-unraid`](https://github.com/crywolf203/lrcget-unraid) |

More templates may be added over time as I build wrappers for apps I actually use.

---

## Repository structure

```text
unraid-templates/
├── ca_profile.xml
├── README.md
├── LICENSE
├── lrcget-icon.png
└── templates/
    └── lrcget.xml
```

### `ca_profile.xml`

This file describes the template repository for Unraid Community Applications.

### `templates/`

Each app gets its own XML template file inside the `templates/` folder.

Example:

```text
templates/lrcget.xml
```

Future templates will follow the same pattern:

```text
templates/example-app.xml
templates/another-app.xml
```

### Icons

App icons are stored in the root of the repository unless a different structure is needed.

Example:

```text
lrcget-icon.png
```

Each template should point to its own icon using a raw GitHub URL.

---

## How to use these templates in Unraid

The preferred method is through **Unraid Community Applications**.

In Unraid:

```text
Apps → Search for the app name → Install
```

If a template is not visible in Community Applications yet, it may still be installable manually by using the XML template or by adding the Docker container directly.

---

## Template philosophy

The templates in this repo are built around a few principles.

### 1. Use apps I actually run

I intend for future Unraid templates in this repo to be wrappers for apps I personally use on my own Unraid server.

This helps keep the templates practical, tested, and grounded in real-world usage.

### 2. Respect upstream developers

Each app belongs to its original developer or project.

This repository only provides Unraid-friendly installation templates or container wrappers.

### 3. Keep paths clear

Templates should clearly define what each path does.

Common examples:

| Container Path | Purpose |
|---|---|
| `/config` | Persistent app configuration and database files |
| `/data` | General app data |
| `/downloads` | Download location |
| `/media` | Media library path |
| `/music` | Music library path |

### 4. Prefer safe defaults

Templates should use defaults that make sense for most Unraid users.

Common examples:

```text
/mnt/user/appdata/appname
/mnt/user/media
/mnt/user/downloads
```

### 5. Explain advanced settings

If a template needs special variables, extra parameters, capabilities, rendering flags, or browser audio settings, those should be documented in the app-specific README or container repository.

For example, the LRCGET template includes `WEB_AUDIO=1` because its browser-based GUI container can stream application audio through the WebUI on port `5800`.

---

## Support

For template-specific issues, such as:

- Unraid path mappings
- Docker environment variables
- WebUI links
- Template XML problems
- Permission issues caused by the template
- Missing or incorrect Community Apps metadata

Open an issue in this repository:

```text
https://github.com/crywolf203/unraid-templates/issues
```

For issues with a specific app, please check the app's upstream project first.

For example, LRCGET application issues should go to:

```text
https://github.com/tranxuanthang/lrcget
```

Container-specific issues for LRCGET for Unraid can go to:

```text
https://github.com/crywolf203/lrcget-unraid/issues
```

---

## Adding future templates

When adding a new Unraid template later, the expected process is:

1. Create or identify the Docker image.
2. Test it locally on Unraid.
3. Create a new XML file under `templates/`.
4. Add an icon if needed.
5. Document the paths, ports, variables, audio behavior, and any extra parameters.
6. Submit or refresh the Community Applications repository.
7. Maintain the template if app behavior changes.

Example future structure:

```text
unraid-templates/
├── ca_profile.xml
├── README.md
├── LICENSE
├── lrcget-icon.png
├── another-app-icon.png
└── templates/
    ├── lrcget.xml
    └── another-app.xml
```

---

## Disclaimer

These templates are unofficial community templates unless explicitly stated otherwise.

All trademarks, project names, logos, and application code belong to their respective owners.

This repository exists only to provide Unraid-friendly templates or wrappers for applications I use and want to share with the Unraid community.

---

## License

This repository is licensed under the MIT License.

The license applies to the template files, documentation, and repository-specific work in this repo.

It does **not** change the license of any upstream application referenced by these templates.
