<p align="center">
  <img src="https://raw.githubusercontent.com/crywolf203/unraid-templates/main/unraid-templates-icon.png" alt="Unraid Templates by crywolf203" width="180">
</p>

<h1 align="center">crywolf203 Unraid Templates</h1>

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

## Table of Contents

- [Overview](#overview)
- [Current Templates](#current-templates)
- [How to Use These Templates in Unraid](#how-to-use-these-templates-in-unraid)
  - [Community Applications](#community-applications)
  - [Manual Template Repository Install](#manual-template-repository-install)
- [Update Tracking in Unraid](#update-tracking-in-unraid)
- [Repository Structure](#repository-structure)
- [Template Philosophy](#template-philosophy)
- [Template Notes](#template-notes)
  - [AMA-Unraid](#ama-unraid)
  - [LRCGET](#lrcget)
  - [PyLrcGet](#pylrcget)
  - [IPTVBoss](#iptvboss)
- [Support](#support)
- [Donations](#donations)
- [Adding Future Templates](#adding-future-templates)
- [Disclaimer](#disclaimer)

---

## Overview

This repository contains Unraid Community Applications XML templates maintained by [`crywolf203`](https://github.com/crywolf203).

The goal of this repo is simple:

> Create clean, useful Unraid templates for applications I personally run, test, and use on my own Unraid server.

These templates are intended to make installation easier for other Unraid users while keeping each setup close to the way the upstream app or Docker image actually works.

---

## Current Templates

| App | Description | Template | Guide / Project | Upstream Project / Image |
|---|---|---|---|---|
| AMA-Unraid | Unraid-focused Automated Music Archiver fork with Deemix Direct, synced lyrics fallback, ReplayGain, Plex/Roon metadata cleanup, high-quality album art, and optional Lidarr/Plex integration. | [`templates/ama-unraid.xml`](templates/ama-unraid.xml) | [`crywolf203/ama-unraid`](https://github.com/crywolf203/ama-unraid) | [`ghcr.io/crywolf203/ama-unraid:latest`](https://github.com/crywolf203/ama-unraid/pkgs/container/ama-unraid) |
| LRCGET | Browser-accessible Unraid wrapper for LRCGET, a tool for downloading synced `.lrc` lyrics for offline music libraries. Includes WebUI audio support with `WEB_AUDIO=1`. | [`templates/lrcget.xml`](templates/lrcget.xml) | See the container README | [`tranxuanthang/lrcget`](https://github.com/tranxuanthang/lrcget) / [`crywolf203/lrcget-unraid`](https://github.com/crywolf203/lrcget-unraid) |
| IPTVBoss | Unraid template for the upstream IPTVBoss Docker image. Uses the full noVNC browser client so copy/paste works directly from the browser WebUI. Includes optional XC Server and cron support. | [`templates/iptvboss.xml`](templates/iptvboss.xml) | [`docs/iptvboss.md`](docs/iptvboss.md) | [`groenator/iptvboss-docker`](https://github.com/groenator/iptvboss-docker) |
| PyLrcGet | Browser-accessible Unraid wrapper for PyLrcGet, a desktop lyrics manager and player. Runs inside LinuxServer Webtop with HTTPS browser access and includes Firefox, Chrome, ffmpeg, mediainfo, and kid3-cli. | [`templates/pylrcget.xml`](templates/pylrcget.xml) | [`docs/pylrcget.md`](docs/pylrcget.md) | [`saitatter/pylrcget`](https://github.com/saitatter/pylrcget) / [`crywolf203/pylrcget-unraid`](https://github.com/crywolf203/pylrcget-unraid) |

More templates may be added over time as I build wrappers for apps I actually use.

---

## How to Use These Templates in Unraid

### Community Applications

The preferred method is through **Unraid Community Applications**.

In Unraid:

```text
Apps → Search for the app name → Install
```

### Manual Template Repository Install

If a template is not visible in Community Applications yet, it can usually still be installed manually by adding this template repository to Unraid.

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

## Update Tracking in Unraid

Templates in this repository are designed to keep Unraid update behavior clear and predictable.

For container image updates:

- Templates use normal Docker image references in the `Repository` field.
- Maintained wrapper images use `:latest` unless a template intentionally pins a version.
- Example: `ghcr.io/crywolf203/ama-unraid:latest`.
- This lets Unraid check whether the local container image is behind the published image.

For template metadata updates:

- Templates include a `TemplateURL` that points back to the raw XML file in this repository.
- Example: `https://raw.githubusercontent.com/crywolf203/unraid-templates/main/templates/ama-unraid.xml`.
- Keeping `TemplateURL` current helps Unraid and Community Applications refresh template metadata such as descriptions, paths, variables, icons, and support/project links.

For support and project links:

- `Support` points to this template repository's Issues page for Unraid-template-specific problems.
- `Project` points to the related app or wrapper repository when available.
- `Registry` points to the Docker image registry or package page when available.

When updating a template, avoid removing these fields unless there is a specific reason:

```xml
<Repository>...</Repository>
<Registry>...</Registry>
<Support>...</Support>
<Project>...</Project>
<TemplateURL>...</TemplateURL>
<Icon>...</Icon>
```

---

## Repository Structure

```text
unraid-templates/
├── README.md
├── LICENSE
├── ca_profile.xml
├── unraid-templates-icon.png
├── ama-unraid-icon.png
├── lrcget-icon.png
├── iptvboss-icon.png
├── docs/
│   ├── iptvboss.md
│   └── pylrcget.md
└── templates/
    ├── ama-unraid.xml
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
templates/ama-unraid.xml
templates/lrcget.xml
templates/iptvboss.xml
templates/pylrcget.xml
```

Future templates should follow the same pattern:

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
ama-unraid-icon.png
lrcget-icon.png
iptvboss-icon.png
```

Each template points to its icon using a raw GitHub URL.

---

## Template Philosophy

The templates in this repo are built around a few principles.

### 1. Use apps I actually run

Templates in this repo are intended to be wrappers for apps I personally use on my own Unraid server.

This helps keep the templates practical, tested, and grounded in real-world usage.

### 2. Respect upstream developers

Each app belongs to its original developer or project.

This repository only provides Unraid-friendly installation templates, Docker wrappers, or additional documentation.

### 3. Keep paths clear

Templates should clearly define what each path does.

Common examples:

| Container Path | Purpose |
|---|---|
| `/config` | Persistent app configuration, scripts, logs, cache, and database files |
| `/data` | General app data |
| `/downloads` | Download location |
| `/downloads-ama` | AMA-Unraid final music library and internal temp working folder |
| `/media` | Media library path |
| `/music` | Music library path |
| `/headless/IPTVBoss` | IPTVBoss persistent app data |

### 4. Prefer safe defaults

Templates should use defaults that make sense for most Unraid users.

Common examples:

```text
/mnt/cache/appdata/appname
/mnt/user/media
/mnt/user/downloads
```

### 5. Explain advanced settings

If a template needs special variables, extra parameters, capabilities, rendering flags, WebUI behavior, browser audio, cron settings, direct download modes, or copy/paste instructions, those details should be documented in the template and, when needed, in the app-specific README or guide.

Examples:

- AMA-Unraid documents Deemix Direct, `MODE=artist` vs `MODE=discography`, conversion settings, and legacy Deemix API options.
- LRCGET uses `WEB_AUDIO=1` so browser audio works through the WebUI.
- IPTVBoss uses `/vnc.html` instead of the lite noVNC URL so browser copy/paste works correctly.
- PyLrcGet uses LinuxServer Webtop HTTPS access on container port `3001`.

### 6. Preserve update metadata

Templates should preserve Unraid/Community Applications metadata so users continue receiving useful update notices and support links.

Important fields include:

```xml
<Repository>...</Repository>
<Registry>...</Registry>
<Support>...</Support>
<Project>...</Project>
<TemplateURL>...</TemplateURL>
<Icon>...</Icon>
```

---

## Template Notes

### AMA-Unraid

AMA-Unraid is provided through a separate Unraid-friendly Docker image:

```text
ghcr.io/crywolf203/ama-unraid:latest
```

The app repository is:

```text
https://github.com/crywolf203/ama-unraid
```

The Unraid template is:

```text
templates/ama-unraid.xml
```

Recommended setup:

```text
DOWNLOAD_CLIENT=deemix_direct
MODE=artist
FORMAT=FLAC
DEEMIX_FALLBACK_BITRATE=true
FORCECONVERT=false
REPLAYGAIN=true
ENABLE_ARTIST_TAG_CLEANUP=true
```

Important paths:

| Container Path | Purpose |
|---|---|
| `/config` | AMA-Unraid appdata, scripts, logs, cache, artist lists, and runtime Deemix Direct config |
| `/downloads-ama` | Final processed music library and AMA-managed `/downloads-ama/temp` working folder |
| `/deemix-config` | Optional Deemix `login.json` folder, only needed when not using `ARL_TOKEN` |
| `/deemix-downloads` | Legacy Deemix API-only downloads folder |

Important mode behavior:

```text
MODE=artist
```

Downloads albums listed directly under the selected artist.

```text
MODE=discography
```

Downloads albums listed under the selected artist plus albums where that artist appears as a contributor or featured artist.

Direct mode does not require a separate Deemix API/WebUI container. Legacy API settings remain available for users who intentionally keep that older workflow.

The AMA-Unraid template keeps the following update metadata:

```xml
<Repository>ghcr.io/crywolf203/ama-unraid:latest</Repository>
<Registry>https://github.com/crywolf203/ama-unraid/pkgs/container/ama-unraid</Registry>
<Project>https://github.com/crywolf203/ama-unraid</Project>
<Support>https://github.com/crywolf203/unraid-templates/issues</Support>
<TemplateURL>https://raw.githubusercontent.com/crywolf203/unraid-templates/main/templates/ama-unraid.xml</TemplateURL>
```

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
- Missing or incorrect Community Applications metadata
- Unraid-specific documentation corrections
- Template update tracking problems

Open an issue in this repository:

```text
https://github.com/crywolf203/unraid-templates/issues
```

For issues with a specific app, please check the app's upstream project first.

### AMA-Unraid links

AMA-Unraid app/container issues:

```text
https://github.com/crywolf203/ama-unraid/issues
```

Unraid template issues:

```text
https://github.com/crywolf203/unraid-templates/issues
```

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

## Adding Future Templates

When adding a new Unraid template later, the expected process is:

1. Create or identify the Docker image.
2. Test it locally on Unraid.
3. Create a new XML file under `templates/`.
4. Add an icon if needed.
5. Add a guide under `docs/` if the app needs extra Unraid-specific explanation.
6. Document paths, ports, variables, WebUI behavior, audio behavior, cron behavior, update behavior, and any extra parameters.
7. Include `Repository`, `Registry`, `Support`, `Project`, `TemplateURL`, and `Icon` metadata where applicable.
8. Submit or refresh the Community Applications repository.
9. Maintain the template if app behavior changes.

Example future structure:

```text
unraid-templates/
├── README.md
├── LICENSE
├── ca_profile.xml
├── unraid-templates-icon.png
├── ama-unraid-icon.png
├── lrcget-icon.png
├── iptvboss-icon.png
├── another-app-icon.png
├── docs/
│   ├── iptvboss.md
│   ├── pylrcget.md
│   └── another-app.md
└── templates/
    ├── ama-unraid.xml
    ├── lrcget.xml
    ├── iptvboss.xml
    ├── pylrcget.xml
    └── another-app.xml
```

---

## Disclaimer

The templates in this repository are community-maintained Unraid helpers.

They may wrap or reference upstream projects, Docker images, and services owned by others. This repository does not claim ownership of those upstream projects.

Use each container only with accounts, media, content, services, and subscriptions you are authorized to access.
