# iMessage Archive on Unraid

iMessage Archive creates local iPhone backups with libimobiledevice and reads Messages from those backups with ReagentX's imessage-exporter. This custom web app adds indexed search, bounded conversation views, dark mode, multi-format downloads, and resumable PDF jobs. It does not extract messages directly from a live iPhone and is not affiliated with Apple or iMazing.

## Install

The template is [templates/imessage-archive.xml](../templates/imessage-archive.xml), using the public Linux amd64 image `ghcr.io/crywolf203/imessage-archive:latest`. Search for `imessage-archive` in Apps after Community Applications has indexed the repository. Until the listing is visible, install the XML privately:

```sh
mkdir -p /boot/config/plugins/community.applications/private/crywolf203
curl --fail --location \
  https://raw.githubusercontent.com/crywolf203/unraid-templates/main/templates/imessage-archive.xml \
  --output /boot/config/plugins/community.applications/private/crywolf203/imessage-archive.xml
```

Refresh Apps and select the private template. Do not install a second instance if Compose already manages your existing archive.

Set a unique administrator password and session secret in the template before applying. Generate the secret in the Unraid terminal with `openssl rand -hex 32`. Web UI port 8087 maps to container port 8080. Open the Web UI and sign in with the configured administrator account.

## Storage

| Container path | Default Unraid host path | Contents |
| --- | --- | --- |
| `/data/backups` | `/mnt/cache/appdata/imessage-archive/backups` | iPhone backups |
| `/data/config` | `/mnt/cache/appdata/imessage-archive/config` | SQLite index, history and settings |
| `/var/lib/lockdown` | `/mnt/cache/appdata/imessage-archive/lockdown` | Device pairing records |
| `/data/exports` | `/mnt/user/iphone-message-archive/exports` | Exported messages, media and ZIP files |
| `/data/pdfs` | `/mnt/user/iphone-message-archive/pdfs` | Generated PDFs |

Choose an array-backed private export share if the cache drive is small. `/mnt/user` does not force array-only placement; verify the share's storage settings. Backups may also need a larger storage path if a first full backup will exceed cache capacity.

For an existing installation, record the actual container mounts and reuse them:

```sh
docker inspect imessage-archive --format '{{range .Mounts}}{{println .Destination "->" .Source}}{{end}}'
```

Changing a mount does not move existing files. Preserve the existing `.env` for Compose, all data folders and pairing records. Stop the old container before switching management between Compose and the Unraid template. Do not allow two instances to write the same archive or compete for the same USB device.

## USB and the first backup

The template uses privileged mode and a `/dev/bus/usb` bind mount for hotplug. This grants broad host access and should only be used on a trusted private server. Only one usbmuxd service should own the connected iPhone.

1. Connect the unlocked iPhone with a data-capable USB cable.
2. Accept **Trust This Computer**, enter the device passcode when requested, and select **Pair device**.
3. Run preflight. Enable encrypted backups with a password you can retain securely.
4. Start a backup. Use normal incremental backups after the initial complete backup.
5. Export HTML to build the searchable library, then select a conversation for PDF/CSV/ZIP downloads.

Charging without a visible device may indicate a charging-only cable, a USB mapping problem or missing trust. `MBErrorDomain/208` means the device is locked. A first backup can take hours. Wi-Fi operation requires existing pairing and a device visible to `idevice_id -n -l`; this template does not guarantee automatic Wi-Fi discovery.

Backup encryption applies to future backups, not to existing files or generated exports. Keep backups, exports, pairing records and settings out of public SMB shares. Never publish actual messages, backup passwords or pairing records in a support issue.

## Performance and Compose proof

The browser loads bounded conversation parts, not an entire giant HTML thread. PDF jobs run on the server in background parts with progress, an estimated time remaining and cancellation. Use **Fast** images or text-only PDF for the quickest result. **Reliable parts** allows completed parts to be reused after interruption.

The [Compose setup and verification guide](https://github.com/crywolf203/imessage-archive/blob/main/docs/compose.md) shows exact installation commands. The [Verify published image workflow](https://github.com/crywolf203/imessage-archive/actions/workflows/verify-image.yml) starts that Compose file, checks healthy startup, search, real PDF/media conversions, CSV/ZIP and browser navigation, and uploads timing results and synthetic-data screenshots. The test uses disposable storage; it is not proof of physical pairing or an actual iPhone backup on every iOS release.

## Community Applications publication

This repository already has an MIT license and `ca_profile.xml`. The template includes the public image, icon, canonical update URL, support/project links and setup requirements. For an initial repository submission or a rescan, use [Community Apps submission](https://ca.unraid.net/submit/new): sign in with Unraid, enter `https://github.com/crywolf203/unraid-templates`, validate and scan, and review any warnings before submitting. If this repository is already registered, use its existing submission rather than creating a duplicate.

The repository already supplies the live [LRCGET listing](https://ca.unraid.net/apps/lrcget-0znc5np1v649pd). Adding this template to the registered repository is the normal path; check the existing repository's status or request a rescan if iMessage Archive does not appear. The [template validation workflow](https://github.com/crywolf203/unraid-templates/actions/workflows/validate-imessage.yml) checks its storage, port, security fields and defaults against the app's Compose files.

Publication and review are controlled by Community Applications. The presence of this XML in GitHub is not a claim that an Apps listing is live. Follow the [current Unraid submission guide](https://ca.unraid.net/submit/help).

## Support

- [Template and path issues](https://github.com/crywolf203/unraid-templates/issues)
- [App, image and export issues](https://github.com/crywolf203/imessage-archive/issues)
- [Source and full documentation](https://github.com/crywolf203/imessage-archive)

Do not expose port 8087 directly to the internet. Use a private LAN, VPN, or authenticated HTTPS reverse proxy.
