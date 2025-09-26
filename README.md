# Daggerheart Dice — ESP32 Web Flasher (Horizontal)

This repo hosts a **browser flasher** for the horizontal build using [ESP Web Tools](https://github.com/esphome/esp-web-tools). It runs on **GitHub Pages** so you can flash without the Arduino IDE.

## Quick start
1. In Arduino IDE, open your **horizontal** sketch → **Sketch → Export compiled Binary**.
2. Copy the generated `.bin` into `firmware/` and rename to **`app.bin`**.
3. Commit and push to the `main` branch.
4. Enable Pages (if not using the provided workflow):
   - Repo **Settings → Pages → Build and deployment → Source: GitHub Actions**.
5. Visit your GitHub Pages URL (e.g. `https://<you>.github.io/<repo>/`) and click **Install**.

> The manifest flashes the **application** at **0x10000** (65536), which is standard when your device already has bootloader + partitions from a previous Arduino upload.

## Advanced: blank chips (bootloader + partitions)
If your board is fully blank and app-only flashing fails, add bootloader & partitions to the manifest and include those binaries next to it:

```jsonc
{
  "name": "Daggerheart Dice (Horizontal)",
  "version": "1.0.0",
  "new_install_prompt_erase": true,
  "builds": [
    {
      "chipFamily": "ESP32",
      "parts": [
        { "path": "firmware/bootloader.bin", "offset": 4096 },   // 0x1000
        { "path": "firmware/partitions.bin", "offset": 32768 },  // 0x8000
        { "path": "firmware/app.bin",        "offset": 65536 }   // 0x10000
      ]
    }
  ]
}
```

Offsets above are common defaults for ESP32 Arduino; adjust if your board package differs.

## Local test (optional)
Open `index.html` directly in Chrome/Edge/Brave (file:// works), or use a local server. Web Serial requires a secure context; file:// and GitHub Pages over HTTPS both qualify in modern Chromium.

## Deploy with GitHub Actions (included)
This repo includes a minimal workflow that publishes the site on every push to `main`:

- `.github/workflows/pages.yml`

If you prefer manual Pages settings, you can remove the workflow and enable Pages with your chosen branch/folder.

---

© Your Name. MIT-licensed snippets.
