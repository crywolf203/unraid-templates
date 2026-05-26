<p align="center">
  <img src="https://raw.githubusercontent.com/crywolf203/unraid-templates/main/unraid-templates-icon.png" alt="Unraid Templates by crywolf203" width="180">
</p>

<p align="center">
  <strong>Personal Unraid Community Applications templates for apps I actually use on my own Unraid server.</strong>
</p>

<p align="center">
  <a href="https://unraid.net">
    <img alt="Unraid" src="https://img.shields.io/badge/Unraid-Templates-f15a24?style=for-the-badge">
  </a>
  <a href="https://github.com/crywolf203/unraid-templates/blob/main/LICENSE">
    <img alt="License" src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge">
  </a>
  <a href="https://buymeacoffee.com/crywolf203">
    <img alt="Buy Me a Coffee" src="https://img.shields.io/badge/Support-Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge">
  </a>
</p>

---

## Overview

This repository contains Unraid Community Applications XML templates maintained by [`crywolf203`](https://github.com/crywolf203).

The goal of this repo is simple:

> Create clean, useful Unraid templates for applications I personally run, test, and use on my own Unraid server.

These templates are intended to make installation easier for other Unraid users while keeping each setup close to the way the upstream app or Docker image actually works.

---

## Important note about these templates

The templates in this repository are generally **wrappers** around other applications or existing Docker images.

That means:

- I do not claim ownership of the upstream apps.
- I do not rebrand the upstream apps as my own.
- I only package, document, or template them for easier Unraid use.
- App-specific bugs should usually be reported to the upstream project.
- Docker, Unraid template, path, permission, WebUI, and documentation issues can be reported here.

Whenever possible, each template links back to the original upstream project.

---

## Current templates

| App | Description | Template | Guide | Upstream Project / Image |
|---|---|---|---|---|
| LRCGET | Browser-accessible Unraid wrapper for LRCGET, a tool for downloading synced `.lrc` lyrics for offline music libraries. Includes WebUI audio support with `WEB_AUDIO=1`. | [`templates/lrcget.xml`](templates/lrcget.xml) | See the container README | [`tranxuanthang/lrcget`](https://github.com/tranxuanthang/lrcget) / [`crywolf203/lrcget-unraid`](https://github.com/crywolf203/lrcget-unraid) |
| IPTVBoss | Unraid template for the upstream IPTVBoss Docker image. Uses the full noVNC browser client so copy/paste works directly from the browser WebUI. Includes optional XC Server and cron support. | [`templates/iptvboss.xml`](templates/iptvboss.xml) | [`docs/iptvboss.md`](docs/iptvboss.md) | [`groenator/iptvboss-docker`](https://github.com/groenator/iptvboss-docker) |
| PyLrcGet | Browser-accessible Unraid wrapper for PyLrcGet, a desktop lyrics manager and player. Runs inside LinuxServer Webtop with HTTPS browser access and includes Firefox, Chrome, ffmpeg, mediainfo, and kid3-cli. | [`templates/pylrcget.xml`](templates/pylrcget.xml) | [`docs/pylrcget.md`](docs/pylrcget.md) | [`saitatter/pylrcget`](https://github.com/saitatter/pylrcget) / [`crywolf203/pylrcget-unraid`](https://github.com/crywolf203/pylrcget-unraid) |

More templates may be added over time as I build wrappers for apps I actually use.

---

## Repository structure

```text
unraid-templates/
├── README.md
├── LICENSE
├── ca_profile.xml
├── unraid-templates-icon.png
├── lrcget-icon.png
├── iptvboss-icon.png
├── docs/
│   ├── iptvboss.md
│   └── pylrcget.md
└── templates/
    ├── lrcget.xml
    ├── iptvboss.xml
    └── pylrcget.xml
```

### `ca_profile.xml`

This file describes the template repository for Unraid Community Applications.

### `templates/`

Each app gets its own XML template file inside the `templates/` folder.

Examples:

```text
templates/lrcget.xml
templates/iptvboss.xml
templates/pylrcget.xml
```

Future templates will follow the same pattern:

```text
templates/example-app.xml
templates/another-app.xml
```

### `docs/`

The `docs/` folder contains app-specific Unraid setup notes when the XML template needs more explanation.

Current guides:

```text
docs/iptvboss.md
docs/pylrcget.md
```

### Icons

App and repository icons are stored in the root of the repository, or referenced from the related container repository when appropriate.

Examples:

```text
unraid-templates-icon.png
lrcget-icon.png
iptvboss-icon.png
```

PyLrcGet currently uses the icon from its container repo:

```text
https://raw.githubusercontent.com/crywolf203/pylrcget-unraid/main/icon.png
```

Each template points to its own icon using a raw GitHub URL.

---

## How to use these templates in Unraid

The preferred method is through **Unraid Community Applications**.

In Unraid:

```text
Apps → Search for the app name → Install
```

If a template is not visible in Community Applications yet, it may still be installable manually by adding this template repository to Unraid.

### Manual template repo install

In Unraid:

```text
Settings → Docker → Advanced View → Template repositories
```

Add:

```text
https://github.com/crywolf203/unraid-templates
```

Then go to:

```text
Docker → Add Container
```

Select the desired template from the template dropdown.

---

## Template philosophy

The templates in this repo are built around a few principles.

### 1. Use apps I actually run

I intend for future Unraid templates in this repo to be wrappers for apps I personally use on my own Unraid server.

This helps keep the templates practical, tested, and grounded in real-world usage.

### 2. Respect upstream developers

Each app belongs to its original developer or project.

This repository only provides Unraid-friendly installation templates, Docker wrappers, or additional documentation.

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
| `/headless/IPTVBoss` | IPTVBoss persistent app data |

### 4. Prefer safe defaults

Templates should use defaults that make sense for most Unraid users.

Common examples:

```text
/mnt/user/appdata/appname
/mnt/user/media
/mnt/user/downloads
```

Where appropriate, users can change host paths to cache-backed locations such as:

```text
/mnt/cache/appdata/appname
```

### 5. Explain advanced settings

If a template needs special variables, extra parameters, capabilities, rendering flags, WebUI behavior, browser audio, cron settings, or copy/paste instructions, those details should be documented in the app-specific README or guide.

Examples:

- LRCGET uses `WEB_AUDIO=1` so browser audio works through the WebUI.
- IPTVBoss uses `/vnc.html` instead of the lite noVNC URL so browser copy/paste works correctly.
- PyLrcGet uses LinuxServer Webtop HTTPS access on container port `3001`.

---

## Notes for current templates

### LRCGET

LRCGET is provided through a separate Unraid-friendly Docker wrapper image:

```text
ghcr.io/crywolf203/lrcget-unraid:latest
```

That image is built and published from:

```text
https://github.com/crywolf203/lrcget-unraid
```

The Unraid template in this repo points to that image.

### PyLrcGet

PyLrcGet is provided through a separate Unraid-friendly Docker wrapper image:

```text
crywolf203/pylrcget-unraid:latest
```

That image is built and published from:

```text
https://github.com/crywolf203/pylrcget-unraid
```

The Unraid template in this repo points to that image and uses LinuxServer Webtop HTTPS access on container port `3001`.

Important WebUI note:

```text
https://[IP]:[PORT:3001]/
```

Inside PyLrcGet, your music folder should be selected as:

```text
/music
```

Detailed guide:

```text
docs/pylrcget.md
```

### IPTVBoss

IPTVBoss uses the upstream Docker image directly:

```text
ghcr.io/groenator/iptvboss-docker:latest
```

The main Unraid-specific improvement is the WebUI URL:

```text
http://[IP]:[PORT:6901]/vnc.html?autoconnect=true&password=iptvboss&resize=scale
```

This opens the full noVNC client instead of the lite client, which allows copy/paste from the browser window.

Detailed guide:

```text
docs/iptvboss.md
```

---

## Support

For template-specific issues, such as:

- Unraid path mappings
- Docker environment variables
- WebUI links
- Template XML problems
- Permission issues caused by the template
- Missing or incorrect Community Apps metadata
- Unraid-specific documentation corrections

Open an issue in this repository:

```text
https://github.com/crywolf203/unraid-templates/issues
```

For issues with a specific app, please check the app's upstream project first.

### LRCGET links

Application-level LRCGET issues:

```text
https://github.com/tranxuanthang/lrcget
```

Container-specific LRCGET for Unraid issues:

```text
https://github.com/crywolf203/lrcget-unraid/issues
```

### PyLrcGet links

Application-level PyLrcGet issues:

```text
https://github.com/saitatter/pylrcget
```

Container-specific PyLrcGet for Unraid issues:

```text
https://github.com/crywolf203/pylrcget-unraid/issues
```

Unraid template issues:

```text
https://github.com/crywolf203/unraid-templates/issues
```

### IPTVBoss links

Upstream IPTVBoss Docker image:

```text
https://github.com/groenator/iptvboss-docker
```

Unraid-specific IPTVBoss template or documentation issues:

```text
https://github.com/crywolf203/unraid-templates/issues
```

---

## Donations

If these Unraid templates help you, you can support my template and container maintenance here:

```text
https://buymeacoffee.com/crywolf203
```

Please also consider supporting the upstream developers of the apps you use. This repository exists because of their work.

---

## Adding future templates

When adding a new Unraid template later, the expected process is:

1. Create or identify the Docker image.
2. Test it locally on Unraid.
3. Create a new XML file under `templates/`.
4. Add an icon if needed.
5. Add a guide under `docs/` if the app needs extra Unraid-specific explanation.
6. Document paths, ports, variables, WebUI behavior, audio behavior, cron behavior, and any extra parameters.
7. Submit or refresh the Community Applications repository.
8. Maintain the template if app behavior changes.

Example future structure:

```text
unraid-templates/
├── README.md
├── LICENSE
├── ca_profile.xml
├── unraid-templates-icon.png
├── lrcget-icon.png
├── iptvboss-icon.png
├── another-app-icon.png
├── docs/
│   ├── iptvboss.md
│   ├── pylrcget.md
│   └── another-app.md
└── templates/
    ├── lrcget.xml
    ├── iptvboss.xml
    ├── pylrcget.xml
    └── another-app.xml
```

---

## Disclaimer

These templates are unofficial community templates unless explicitly stated otherwise.

All trademarks, project names, logos, Docker images, and application code belong to their respective owners.

This repository exists only to provide Unraid-friendly templates, documentation, and wrappers for applications I use and want to share with the Unraid community.

---

## License

This repository is licensed under the MIT License.

The license applies to the template files, documentation, and repository-specific work in this repo.

It does **not** change the license of any upstream application or Docker image referenced by these templates.
