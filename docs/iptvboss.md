<p align="center">
  <img src="https://raw.githubusercontent.com/crywolf203/unraid-templates/main/iptvboss-icon.png" alt="IPTVBoss for Unraid icon" width="180">
</p>

<p align="center">
  <strong>Unraid-focused setup guide for the upstream IPTVBoss Docker container.</strong>
</p>

---

## Overview

This guide explains how to run the upstream IPTVBoss Docker container on Unraid using the Community Applications template maintained in this repository.

The Docker image itself is maintained upstream by [`groenator/iptvboss-docker`](https://github.com/groenator/iptvboss-docker). This repository does **not** rebuild, repackage, or replace that image. It only provides an Unraid-friendly XML template and Unraid-specific usage notes.

This documentation was updated after upstream PR [`#219`](https://github.com/groenator/iptvboss-docker/pull/219) was approved and merged. That PR added the Firefox default-browser fix to the upstream Docker image, so Dropbox authorization links should open in Mozilla Firefox automatically when no user browser preference exists.

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

## Why this guide was updated

This guide was updated after upstream PR [`#219`](https://github.com/groenator/iptvboss-docker/pull/219) was approved and merged into `groenator/iptvboss-docker`.

Before that upstream fix, this Unraid template temporarily documented a `/headless/.config` desktop-config workaround so users could manually persist the Firefox default-browser setting for Dropbox authorization.

After PR `#219`, the upstream Docker image sets Mozilla Firefox as the default XFCE/XDG browser when no user preference exists. Because of that, the Unraid template no longer needs to map `/headless/.config`.

The Unraid template still keeps the full noVNC WebUI URL because that is what enables the browser clipboard/copy-paste experience:

```text
/vnc.html?autoconnect=true&password=iptvboss&resize=scale
```

---

## What this template does

The Unraid template points directly to the upstream image:

```text
ghcr.io/groenator/iptvboss-docker:latest
```

It configures the important Unraid pieces:

- Persistent IPTVBoss app data
- Browser noVNC WebUI
- Full noVNC client URL for copy/paste support
- Optional native VNC port
- Optional XC Server port
- PUID/PGID permissions
- Cron schedule for automated no-GUI runs
- VNC resolution, color depth, and password variables

The main Unraid-specific improvement is the WebUI URL.

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
/mnt/cache/appdata/iptvboss
```

Container path:

```text
/headless/IPTVBoss
```

The old `/headless/.config` workaround is no longer needed because upstream PR [`#219`](https://github.com/groenator/iptvboss-docker/pull/219) added the Mozilla Firefox default-browser fix to the image.

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
| `/headless/IPTVBoss` | `/mnt/cache/appdata/iptvboss` | Persistent IPTVBoss app data |

Do not add a separate `/headless/.config` mapping for normal installs. That workaround was only needed before the upstream Firefox default-browser fix.

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

## Dropbox authorization

The upstream IPTVBoss Docker image now sets Mozilla Firefox as the default XFCE/XDG browser when no user preference exists.

If Dropbox authorization does not open, verify the browser setting inside the IPTVBoss desktop:

```text
Applications > Settings > Default Applications > Internet > Web Browser
```

It should be:

```text
Mozilla Firefox
```

You can also verify from Unraid Terminal:

```bash
docker exec -it IPTVBoss bash -lc '
cat /headless/.config/xfce4/helpers.rc 2>/dev/null || true
cat /headless/.config/mimeapps.list 2>/dev/null || true
'
```

Expected values include:

```text
WebBrowser=firefox
x-scheme-handler/http=firefox.desktop
x-scheme-handler/https=firefox.desktop
```

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

Recommended host path:

```text
/mnt/cache/appdata/iptvboss
```

If IPTVBoss does not start, check appdata permissions first.

From Unraid Terminal:

```bash
mkdir -p /mnt/cache/appdata/iptvboss
chown -R 99:100 /mnt/cache/appdata/iptvboss
chmod -R 775 /mnt/cache/appdata/iptvboss
```

Then restart the container.

---

## Manual docker run test

This is a clean local test command for Unraid Terminal.

```bash
docker rm -f iptvboss-test 2>/dev/null || true

mkdir -p /mnt/cache/appdata/iptvboss-test
chown -R 99:100 /mnt/cache/appdata/iptvboss-test
chmod -R 775 /mnt/cache/appdata/iptvboss-test

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

### Dropbox authorization still fails

Verify that the upstream image has been updated and that Firefox is set as the default browser:

```bash
docker pull ghcr.io/groenator/iptvboss-docker:latest

docker exec -it IPTVBoss bash -lc '
cat /headless/.config/xfce4/helpers.rc 2>/dev/null || true
cat /headless/.config/mimeapps.list 2>/dev/null || true
'
```

Expected values:

```text
WebBrowser=firefox
x-scheme-handler/http=firefox.desktop
x-scheme-handler/https=firefox.desktop
```

If those values are missing, recreate the container after pulling the latest image.

### IPTVBoss does not start

Check appdata permissions:

```bash
chown -R 99:100 /mnt/cache/appdata/iptvboss
chmod -R 775 /mnt/cache/appdata/iptvboss
```

Also confirm the path maps to:

```text
/headless/IPTVBoss
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
