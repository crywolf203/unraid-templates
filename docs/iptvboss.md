<p align="center">
  <img src="https://raw.githubusercontent.com/crywolf203/unraid-templates/main/iptvboss-icon.png" alt="IPTVBoss for Unraid icon" width="180">
</p>

<p align="center">
  <strong>Unraid-focused setup guide for the upstream IPTVBoss Docker container.</strong>
</p>

---

## Overview

This guide explains how to run the upstream IPTVBoss Docker container on Unraid using the Community Applications template maintained in this repository.

The Docker image itself is maintained upstream by [`groenator/iptvboss-docker`](https://github.com/groenator/iptvboss-docker). This repository does **not** rebuild, repackage, or replace that image. It only provides an Unraid-friendly XML template and clearer Unraid-specific usage notes.

```text
Upstream IPTVBoss Docker image
ghcr.io/groenator/iptvboss-docker:latest
        │
        │ Unraid XML template
        ▼
Browser-accessible IPTVBoss desktop
        │
        │ Full noVNC client
        ▼
Copy/paste works from the browser
```

---

## What this template does

The Unraid template points directly to the upstream image:

```text
ghcr.io/groenator/iptvboss-docker:latest
```

It configures the important Unraid pieces:

- Persistent IPTVBoss app data
- Persistent desktop configuration
- Browser noVNC WebUI
- Full noVNC client URL for copy/paste support
- Optional native VNC port
- Optional XC Server port
- PUID/PGID permissions
- Cron schedule for automated no-GUI runs
- VNC resolution, color depth, and password variables
- Dropbox authorization/default-browser guidance

The main improvement in this template is the WebUI URL.

Instead of opening the lite noVNC client:

```text
http://SERVER-IP:6901/?password=iptvboss
```

the template opens the full noVNC client:

```text
http://SERVER-IP:6901/vnc.html?autoconnect=true&password=iptvboss&resize=scale
```

The full noVNC client includes the browser-side controls needed for better keyboard, clipboard, and copy/paste behavior.

---

## Installation

Install from Unraid Community Applications when available.

Search for:

```text
IPTVBoss
```

If installing manually from this template repo, the XML template is here:

```text
templates/iptvboss.xml
```

---

## Recommended Unraid settings

### Repository

```text
ghcr.io/groenator/iptvboss-docker:latest
```

### WebUI

Use this exact WebUI format in Unraid:

```text
http://[IP]:[PORT:6901]/vnc.html?autoconnect=true&password=iptvboss&resize=scale
```

In the XML template, the same line must use escaped ampersands:

```xml
<WebUI>http://[IP]:[PORT:6901]/vnc.html?autoconnect=true&amp;password=iptvboss&amp;resize=scale</WebUI>
```

### Appdata path

Recommended host path:

```text
/mnt/user/appdata/iptvboss
```

Cache-preferred example:

```text
/mnt/cache/appdata/iptvboss
```

Container path:

```text
/headless/IPTVBoss
```

### Desktop config path

Recommended host path:

```text
/mnt/user/appdata/iptvboss-desktop-config
```

Cache-preferred example:

```text
/mnt/cache/appdata/iptvboss-desktop-config
```

Container path:

```text
/headless/.config
```

This path preserves desktop settings such as the default web browser selection used for Dropbox authorization.

---

## Ports

| Container Port | Purpose | Required |
|---:|---|:---:|
| `6901` | Browser noVNC WebUI | Yes |
| `5901` | Native VNC client access | Optional |
| `8001` | IPTVBoss XC Server | Optional, only useful when `XC_SERVER=true` |

### Browser WebUI

The browser WebUI uses container port:

```text
6901
```

The recommended WebUI path is:

```text
/vnc.html?autoconnect=true&password=iptvboss&resize=scale
```

### Native VNC

Native VNC uses:

```text
5901
```

Most Unraid users should not need a separate VNC app because the full noVNC browser client works. Native VNC is only included as a fallback.

### XC Server

XC Server uses:

```text
8001
```

It only starts when:

```text
XC_SERVER=true
```

When enabled, access it at:

```text
http://YOUR-UNRAID-IP:8001
```

or whatever host port you mapped to container port `8001`.

---

## Paths

| Container Path | Recommended Host Path | Purpose |
|---|---|---|
| `/headless/IPTVBoss` | `/mnt/user/appdata/iptvboss` | Persistent IPTVBoss app data |
| `/headless/.config` | `/mnt/user/appdata/iptvboss-desktop-config` | Persistent desktop/VNC configuration, including default browser settings |

### Why `/headless/.config` matters

Dropbox authorization may require changing the desktop default web browser to Mozilla.

That setting is stored in the desktop configuration area. Mapping `/headless/.config` allows the setting to survive container restarts, updates, and recreation.

---

## Environment variables

| Variable | Recommended Unraid Value | Description |
|---|---:|---|
| `PUID` | `99` | User ID used by the container. On Unraid, `99` is commonly the `nobody` user. |
| `PGID` | `100` | Group ID used by the container. On Unraid, `100` is commonly the `users` group. |
| `TZ` | `America/New_York` | Container timezone. Change this to match your location. |
| `VNC_PW` | `iptvboss` | Password for the browser noVNC and native VNC sessions. |
| `VNC_RESOLUTION` | `1920x1080` | Desktop resolution inside the VNC session. |
| `VNC_COL_DEPTH` | `24` | VNC color depth. |
| `CRON_SCHEDULE` | `0 3,15 * * *` | Cron schedule for automated IPTVBoss no-GUI runs. |
| `XC_SERVER` | `true` | Starts IPTVBoss XC Server on container port `8001`. |
| `CRONITOR_API_KEY` | blank | Optional Cronitor API key for monitoring cron jobs. |
| `CRONITOR_SCHEDULE_NAME` | blank | Optional custom Cronitor schedule name. |

---

## Important password note

The default template WebUI assumes:

```text
VNC_PW=iptvboss
```

That is why this URL works automatically:

```text
http://[IP]:[PORT:6901]/vnc.html?autoconnect=true&password=iptvboss&resize=scale
```

If you change `VNC_PW`, then the WebUI URL password also needs to match.

For example, if you change:

```text
VNC_PW=mysecretpassword
```

then your WebUI URL should become:

```text
http://[IP]:[PORT:6901]/vnc.html?autoconnect=true&password=mysecretpassword&resize=scale
```

If the WebUI still contains the old password, noVNC may fail to auto-connect or may ask you to enter the password manually.

---

## Copy and paste from the browser

Use the full noVNC client:

```text
/vnc.html?autoconnect=true&password=iptvboss&resize=scale
```

Do **not** use the lite URL for normal Unraid use:

```text
/?password=iptvboss
```

The `/vnc.html` page is the full noVNC client.

### Copy/paste workflow

1. Open the IPTVBoss WebUI from Unraid.
2. Confirm the URL includes `/vnc.html`.
3. Click the noVNC side toolbar.
4. Open the clipboard panel.
5. Paste text into the noVNC clipboard box.
6. Click inside the IPTVBoss field.
7. Press `Ctrl + V`.

This avoids needing a separate desktop VNC app for setup.

---

## Dropbox authorization / default browser fix

When setting up Dropbox authorization inside IPTVBoss, the app may show an error similar to:

```text
Failed to execute default web browser
```

This happens because IPTVBoss may try to open the Dropbox authorization link with Debian Sensible Browser, but that browser handler is not available in the container.

### Fix inside the IPTVBoss desktop

Open IPTVBoss through the browser WebUI.

Then inside the IPTVBoss desktop, go to:

```text
Applications → Settings → Default Applications
```

In the **Internet** section, set **Web Browser** to:

```text
Mozilla
```

Then retry Dropbox authorization inside IPTVBoss.

### Why the template includes Desktop Config

The Unraid template includes this advanced path:

```text
/headless/.config
```

mapped to:

```text
/mnt/user/appdata/iptvboss-desktop-config
```

This allows the desktop default-application setting to persist across container restarts, updates, and recreates.

Without this persistent desktop config path, the Mozilla default-browser setting may need to be set again after recreating the container.

### If the Dropbox authorization link still does not open

Use the full noVNC WebUI URL:

```text
/vnc.html?autoconnect=true&password=iptvboss&resize=scale
```

Then use the noVNC clipboard panel to copy the Dropbox authorization link and paste it into the Mozilla browser inside the IPTVBoss desktop.

Do not use the lite noVNC URL:

```text
/?password=iptvboss
```

The full `/vnc.html` client is recommended because browser copy/paste works properly there.

---

## Cron schedule

The container can run IPTVBoss no-GUI tasks automatically using cron.

Default template value:

```text
0 3,15 * * *
```

That means it runs daily at:

```text
3:00 AM
3:00 PM
```

Cron uses the timezone set by:

```text
TZ
```

### Common cron examples

| Schedule | Meaning |
|---|---|
| `0 3,15 * * *` | Daily at 3 AM and 3 PM |
| `0 3 * * *` | Daily at 3 AM |
| `0 */6 * * *` | Every 6 hours |
| `30 2 * * *` | Daily at 2:30 AM |

---

## XC Server

To enable XC Server:

```text
XC_SERVER=true
```

Expose/map container port:

```text
8001
```

Then open:

```text
http://YOUR-UNRAID-IP:8001
```

If you do not use XC Server, you can set:

```text
XC_SERVER=false
```

and remove or ignore the `8001` port mapping.

---

## Appdata and permissions

The upstream container expects app data at:

```text
/headless/IPTVBoss
```

The host path must be writable by the container user.

Recommended Unraid values:

```text
PUID=99
PGID=100
```

Recommended host paths:

```text
/mnt/user/appdata/iptvboss
/mnt/user/appdata/iptvboss-desktop-config
```

Cache-preferred host paths:

```text
/mnt/cache/appdata/iptvboss
/mnt/cache/appdata/iptvboss-desktop-config
```

If IPTVBoss does not start, check appdata permissions first.

From Unraid Terminal:

```bash
mkdir -p /mnt/cache/appdata/iptvboss
mkdir -p /mnt/cache/appdata/iptvboss-desktop-config
chown -R 99:100 /mnt/cache/appdata/iptvboss /mnt/cache/appdata/iptvboss-desktop-config
chmod -R 775 /mnt/cache/appdata/iptvboss /mnt/cache/appdata/iptvboss-desktop-config
```

Then restart the container.

---

## Manual docker run test

This is a clean local test command for Unraid Terminal.

```bash
docker rm -f iptvboss-test 2>/dev/null || true

mkdir -p /mnt/cache/appdata/iptvboss-test
mkdir -p /mnt/cache/appdata/iptvboss-test-desktop-config
chown -R 99:100 /mnt/cache/appdata/iptvboss-test /mnt/cache/appdata/iptvboss-test-desktop-config
chmod -R 775 /mnt/cache/appdata/iptvboss-test /mnt/cache/appdata/iptvboss-test-desktop-config

docker run -d \
  --name iptvboss-test \
  --net bridge \
  -l net.unraid.docker.webui='http://[IP]:[PORT:6901]/vnc.html?autoconnect=true&password=iptvboss&resize=scale' \
  -p 6902:6901 \
  -p 5902:5901 \
  -p 8002:8001 \
  -e PUID=99 \
  -e PGID=100 \
  -e TZ=America/New_York \
  -e CRON_SCHEDULE="0 3,15 * * *" \
  -e XC_SERVER=true \
  -e VNC_PW=iptvboss \
  -e VNC_RESOLUTION=1920x1080 \
  -e VNC_COL_DEPTH=24 \
  -v /mnt/cache/appdata/iptvboss-test:/headless/IPTVBoss \
  -v /mnt/cache/appdata/iptvboss-test-desktop-config:/headless/.config \
  ghcr.io/groenator/iptvboss-docker:latest
```

Open:

```text
http://YOUR-UNRAID-IP:6902/vnc.html?autoconnect=true&password=iptvboss&resize=scale
```

---

## Docker Compose example

```yaml
services:
  iptvboss:
    image: ghcr.io/groenator/iptvboss-docker:latest
    container_name: iptvboss
    restart: unless-stopped
    ports:
      - "6901:6901"
      - "5901:5901"
      - "8001:8001"
    environment:
      PUID: "99"
      PGID: "100"
      TZ: "America/New_York"
      CRON_SCHEDULE: "0 3,15 * * *"
      XC_SERVER: "true"
      VNC_PW: "iptvboss"
      VNC_RESOLUTION: "1920x1080"
      VNC_COL_DEPTH: "24"
    volumes:
      - /mnt/user/appdata/iptvboss:/headless/IPTVBoss
      - /mnt/user/appdata/iptvboss-desktop-config:/headless/.config
```

Open the full browser noVNC client:

```text
http://YOUR-UNRAID-IP:6901/vnc.html?autoconnect=true&password=iptvboss&resize=scale
```

---

## Troubleshooting

### Copy/paste does not work in the browser

Make sure you are using:

```text
/vnc.html?autoconnect=true&password=iptvboss&resize=scale
```

not:

```text
/?password=iptvboss
```

The `/vnc.html` page is the full noVNC client.

### WebUI does not auto-connect

Check that the WebUI password matches:

```text
VNC_PW
```

Default:

```text
iptvboss
```

If you changed `VNC_PW`, update the WebUI URL or enter the password manually.

### Dropbox authorization fails with default browser error

Set the desktop default browser to Mozilla:

```text
Applications → Settings → Default Applications → Internet → Web Browser → Mozilla
```

The template includes a persistent `/headless/.config` path so this setting can survive updates and container recreation.

If you changed or removed the Desktop Config path, you may need to set Mozilla again after recreating the container.

### IPTVBoss does not start

Check appdata permissions:

```bash
chown -R 99:100 /mnt/cache/appdata/iptvboss /mnt/cache/appdata/iptvboss-desktop-config
chmod -R 775 /mnt/cache/appdata/iptvboss /mnt/cache/appdata/iptvboss-desktop-config
```

Also confirm the paths map to:

```text
/headless/IPTVBoss
/headless/.config
```

### XC Server does not open

Confirm:

```text
XC_SERVER=true
```

Confirm port `8001` is mapped.

Then open:

```text
http://YOUR-UNRAID-IP:8001
```

### Native VNC does not connect

Native VNC uses port:

```text
5901
```

Use the same password as:

```text
VNC_PW
```

Most users should use the browser WebUI instead.

---

## Updating

This template uses the upstream image:

```text
ghcr.io/groenator/iptvboss-docker:latest
```

When the upstream image is updated, Unraid should show an available Docker update.

This template does not build or publish a separate image.

---

## Credits

The IPTVBoss Docker image is maintained by the upstream project:

```text
https://github.com/groenator/iptvboss-docker
```

This repository only provides an Unraid Community Applications template and Unraid-focused documentation.

---

## Disclaimer

This is an unofficial Unraid template for the upstream IPTVBoss Docker image.

For issues with the Docker image or IPTVBoss behavior, check the upstream project first.

For Unraid template issues, WebUI URL improvements, port mappings, or documentation corrections, use this repository.
