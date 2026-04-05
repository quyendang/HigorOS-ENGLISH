# HigorOS English WebUI

English-translated WebUI dashboard for **Hiveton H5000M** 5G CPE running firmware **HiGoROS-H5000M-1-26-03-25-02.bin**.

All Chinese text in the original web interface has been translated to English. The router functionality is unchanged — only display strings were modified.

---

## Requirements

- Hiveton H5000M router (firmware: HiGoROS-H5000M-1-26-03-25-02.bin)
- SSH access to the router (root)
- `scp` and `ssh` available on your computer
- Default router IP: `192.168.88.1`

---

## Installation

### Step 1 — Backup the original WebUI

Before overwriting, copy the entire original WebUI folder to a backup folder:

```bash
ssh root@192.168.88.1 "cp -a /www/higoros /www/higoros.bak"
```

This creates `/www/higoros.bak/` as a **folder** containing all original files. Verify it exists:

```bash
ssh root@192.168.88.1 "ls /www/higoros.bak"
```

---

### Step 2 — Upload the English WebUI

Clone or download this repository to your computer, then upload it to the router.

**On macOS / Linux:**

```bash
scp -O -r /path/to/HigorOS-ENGLISH/ root@192.168.88.1:/www/higoros/
```

Replace `/path/to/HigorOS-ENGLISH/` with the actual path where you cloned/downloaded this repo.

**Example (if downloaded to Desktop):**

```bash
scp -O -r ~/Desktop/HigorOS-ENGLISH/ root@192.168.88.1:/www/higoros/
```

> **Note:** The `-O` flag is required because this router does not have an SFTP server — `scp` falls back to legacy SCP protocol.

---

### Step 3 — Fix file permissions

After uploading, set correct permissions:

```bash
ssh root@192.168.88.1 "find /www/higoros -type d -exec chmod 755 {} \; && find /www/higoros -type f -exec chmod 644 {} \;"
```

---

### Step 4 — Clear browser cache

The WebUI uses a Service Worker (PWA) that aggressively caches files. After uploading, you **must** clear the browser cache, otherwise the old Chinese version will still be served.

**Option A — Use a private/incognito window** (easiest):
Open your browser in incognito mode and navigate to `http://192.168.88.1`

**Option B — Unregister the Service Worker manually:**
1. Open `http://192.168.88.1` in your browser
2. Press `F12` to open DevTools
3. Go to **Application** → **Service Workers**
4. Click **Unregister**
5. Reload the page (`Ctrl+Shift+R` / `Cmd+Shift+R`)

---

## Restore original Chinese version

If you need to revert to the original Chinese WebUI:

```bash
ssh root@192.168.88.1 "rm -rf /www/higoros && cp -a /www/higoros.bak /www/higoros"
```

Verify restore:

```bash
ssh root@192.168.88.1 "ls /www/higoros"
```

Then clear your browser cache again (see Step 4 above) and reload `http://192.168.88.1`.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| Still seeing Chinese after upload | Clear browser cache / use incognito window |
| `scp: Connection closed` error | Make sure to use `scp -O` (capital letter O, not zero) |
| Permission denied on upload | Confirm SSH login works: `ssh root@192.168.88.1` |
| `SyntaxError` in browser console | Service Worker is serving old cached files — unregister SW via DevTools |
| Router not reachable at 192.168.88.1 | Connect to router's WiFi or LAN port first |

---

## What was changed

- All Chinese UI text translated to English across 17 JavaScript bundle files
- `index.html`: language tag updated (`zh-CN` → `en`), offline message translated
- `manifest.json` / `manifest.webmanifest`: PWA metadata translated
- `sw.js`: Service Worker updated with revision hashes for all modified files + cache invalidation on update

## Compatibility

| Item | Value |
|---|---|
| Device | Hiveton H5000M |
| Firmware | HiGoROS-H5000M-1-26-03-25-02.bin |
| WebUI path on router | `/www/higoros` |
| Default router IP | `192.168.88.1` |
