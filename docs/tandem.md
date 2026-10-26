<p align="center">
  <img src="https://raw.githubusercontent.com/crywolf203/unraid-templates/main/tandem-icon.png" alt="Tandem icon" width="180">
</p>

<p align="center">
  <strong>Unraid setup guide for Tandem, a self-hosted co-parenting app.</strong>
</p>

---

## Overview

[Tandem](https://github.com/crywolf203/tandem) keeps what two co-parents share in one place on your own server:

- parenting-time calendar, with swap requests both parents must accept;
- permanent, tamper-evident messages;
- shared expenses with receipts;
- school calendars;
- the child's health cards and documents;
- optional voice calls, recorded only when both parents agree.

It installs on iPhone and Android from the browser like a native app.

```text
ghcr.io/crywolf203/tandem:latest
        │
        │ One container · one SQLite file in /data
        ▼
Tandem web app on port 4380
        │
        │ HTTPS via your reverse proxy (needed to install it on phones)
        ▼
iPhone / Android home-screen app
```

The full feature list, every setting and troubleshooting are in the [Tandem README](https://github.com/crywolf203/tandem#readme). This guide covers the Unraid side.

---

## Install with the template

1. **Apps** → search **Tandem** → **Install**.

   If it isn't listed yet, add this template repository instead: **Settings → Docker → Template repositories** → `https://github.com/crywolf203/unraid-templates`, then **Docker → Add Container** → **Tandem**.
2. Check **Data** (`/mnt/user/appdata/tandem` by default).
3. Set **Public URL** if you'll use Tandem away from home, e.g. `https://tandem.example.com`.
4. **Apply**, then open the WebUI. The first person to open Tandem creates the family and invites the other parent from **More → Invite someone**.

| Container path | Default host path | Purpose |
|---|---|---|
| `/data` | `/mnt/user/appdata/tandem` | `tandem.db` (accounts, schedule, messages, expenses, calls) and `files/` (documents, card photos, receipts, call recordings). Back this folder up. |

| Container port | Default | Purpose |
|---:|---:|---|
| `4380` | `4380` | Web app and API |

---

## HTTPS and phones

Phones only install Tandem as an app over HTTPS. Point your reverse proxy at the container:

| Proxy | Forward to |
|---|---|
| Nginx Proxy Manager | `http://UNRAID-IP:4380` |
| SWAG | `http://UNRAID-IP:4380` |
| Cloudflare Tunnel | `http://UNRAID-IP:4380` |

Then set **Public URL** to that address and **Behind a reverse proxy** (`TRUST_PROXY`) to `true`.

---

## Settings in the template

The template shows the everyday settings; the rest are under **Show more settings**.

| Group | Settings | Notes |
|---|---|---|
| Access | `PUBLIC_URL`, `ALLOW_SIGNUPS`, `TRUST_PROXY`, `SECURE_COOKIES`, `SESSION_DAYS` | Sign-up is invite-only after the first family by default. |
| Files | `PUID`, `PGID`, `UMASK`, `TZ` | Keep `99` / `100` on Unraid. |
| School calendars | `FEED_REFRESH_HOURS`, `ALLOW_PRIVATE_FEEDS` | Leave `ALLOW_PRIVATE_FEEDS=false` unless a calendar file is on your LAN. |
| Weather | `WEATHER_PROVIDER`, `WEATHER_UNITS`, `ZIP_CENTROIDS_FILE` | `auto` uses the National Weather Service in the US. |
| Receipt reading | `RECEIPT_AI_URL`, `RECEIPT_AI_MODEL`, `RECEIPT_AI_KEY` | Optional. Any OpenAI-compatible vision API, including Ollama on your own server. All blank = off. |
| Calls | `LIVEKIT_URL`, `LIVEKIT_API_URL`, `LIVEKIT_API_KEY`, `LIVEKIT_API_SECRET`, `RECORDINGS_DIR` | Optional. Blank `LIVEKIT_URL` = calls off. See below. |

Every setting is described in the [Tandem README](https://github.com/crywolf203/tandem#environment-variables).

---

## Optional: calls and local AI

Tandem can run extra services next to the app:

- **Calls:** LiveKit, Egress for recording, and Redis.
- **Local AI:** Ollama for reading receipts, and Whisper for future call transcripts.

These need several containers wired together, so they install with **Docker Compose** rather than this single template. On Unraid, use the terminal or the **Compose Manager** plugin.

```bash
mkdir -p /mnt/user/appdata/tandem && cd /mnt/user/appdata/tandem
curl -fsSLO https://raw.githubusercontent.com/crywolf203/tandem/main/docker-compose.yml
curl -fsSLO https://raw.githubusercontent.com/crywolf203/tandem/main/setup.sh && chmod +x setup.sh
./setup.sh
```

`setup.sh` asks which add-ons to turn on, then:

- creates the call server's key and secret;
- picks GPU or CPU (install the **Nvidia Driver** plugin first if you have a GPU);
- writes `.env`.

Then `docker compose up -d` starts Tandem and the add-ons together.

Add-on data goes in `/mnt/user/appdata/tandem-addons`. Use Compose *or* this template for Tandem, not both, so there's only one Tandem container on the data folder.

Calls also need two things outside Unraid; `setup.sh` prints the exact values:

1. **Reverse proxy:** a host name such as `calls.example.com` → `http://UNRAID-IP:7880`, with WebSockets on. A Cloudflare Tunnel works for this part.
2. **Router port forwards, straight to Unraid** (not through a tunnel): UDP `50000–50100` for call audio, and TCP `7881` as a fallback.

If your internet provider uses CGNAT (no public IP of your own), port forwarding can't work. Run the calls add-on on a small VPS instead.

Recording happens only when both parents turn it on in **More → Calls**, and both phones show it during the call.

---

## Updating

On the `latest` tag, Tandem updates through Unraid's normal **Check for Updates**. Database changes apply automatically on start.

An image update does not add new fields to a template you already saved. When a release adds a setting, it's listed in the release notes. Add it by editing the container, or remove and re-add the container from **Apps → Previous Apps** to pick up the new template.

---

## Backups

Everything is in the **Data** folder. Include `/mnt/user/appdata/tandem` in the Appdata Backup plugin schedule. It stops the container, copies the folder and restarts.

---

## Support

- Template problems: [unraid-templates issues](https://github.com/crywolf203/unraid-templates/issues)
- App problems and feature requests: [Tandem issues](https://github.com/crywolf203/tandem/issues)

Tandem's reports are factual exports of what's stored. Tandem is not legal advice.
