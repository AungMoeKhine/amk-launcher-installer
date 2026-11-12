# AMK Multi-Boot App Launcher — Web Installer for ESP32 CYD

An ultra-sleek, browser-based web installer for the **AMK Multi-Boot App Launcher** on ESP32 Cheap Yellow Displays (CYD), powered by [ESP Web Tools](https://esphome.github.io/esp-web-tools/) and the Web Serial API.

> **Developer:** Aung Moe Khine  
> **Display Library:** `AMK_CYD_Universal_TFT` (Hardware auto-detection & frozen color standard)

---

## 🌟 Key Features

- **Hardware-Level Dual-Boot:** Boots into the launcher on every startup; double-tap anytime during the 5-second countdown to switch apps, or let it auto-launch your last selected app.
- **MicroSD Card Carousel:** Simply drop any standard ESP32 compiled `.bin` into `/apps/` on a FAT32 MicroSD card.
- **Universal Hardware Support:** Powered by `AMK_CYD_Universal_TFT`, supporting all ESP32 & ESP32-S3 boards (2.4", 2.8", 3.2", 3.5", 4.0", 4.3", 5.0", and 7.0"), with both **Resistive (XPT2046)** and **Capacitive (GT911, CST820, FT6336U)** touch, and USB-C (ESP32-32E) & Micro-USB variants. Compatible with CYD, Guition, Elecrow CrowPanel, Sunton, and Waveshare displays.
- **Light Sensor (LDR) Auto-Dimming:** Smooth moving average ambient light brightness adjustment.
- **Smart Sleep:** Powers down backlight after 2 minutes of inactivity; tap to wake.
- **Emergency Recovery:** Hold down the **BOOT button (GPIO 0)** during power-on/reset to force-boot back to the launcher menu.
- **Interactive Virtual Aquarium:** Built-in digital aquarium canvas widget included directly on the page!

---

## 🚀 How to Deploy on GitHub Pages

1. **Push this folder to a GitHub repository:**
   ```bash
   git init
   git add .
   git commit -m "Initial commit for AMK Multi-Boot Launcher Web Installer"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo-name>.git
   git push -u origin main
   ```
2. **Enable GitHub Pages:**
   - In your GitHub repository, navigate to **Settings** > **Pages**.
   - Under **Build and deployment** > **Branch**, choose `main` and `/ (root)`.
   - Click **Save**.
3. **Visit Your Live Installer:**
   - Your installer will be accessible via HTTPS at `https://<your-username>.github.io/<your-repo-name>/`.
   *(Note: Web Serial requires an HTTPS connection or localhost to communicate with USB COM ports).*

---

## 🛠️ Flash Memory Partition Table Layout (4MB Flash)

The installer flashes `AMK_Launcher_merged.bin` at offset `0x0`:

| Name | Type | SubType | Flash Offset | Size | Purpose |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Bootloader** | `bootloader` | — | `0x001000` | 28 KB | KeyBoot interceptor bootloader |
| **Partitions** | `partition` | — | `0x008000` | 3 KB | AMK Custom Partition Table |
| **otadata** | `data` | `ota` | `0x00E000` | 8 KB | OTA boot partition selector |
| **test** | `app` | `test` | `0x010000` | 768 KB | AMK Multi-Boot Launcher Application |
| **app0** | `app` | `ota_0` | `0x0D0000` | 1,920 KB | Target user application slot (flashed from SD card) |
| **spiffs** | `data` | `spiffs` | `0x2B0000` | 1,344 KB | Dedicated SPIFFS storage for OSM maps, fonts, and assets |

---

## 📁 MicroSD Card Organization & Included Apps

Format your MicroSD card with **FAT32**. Extract the starter kit `AMK_CYD_SD_Card_Files.zip` directly to your MicroSD card root:

```text
SD Card Root (FAT32)
├── apps/
│   ├── FlightRadar.bin        <- Live Aircraft Radar Tracker
│   ├── FlipClock.bin          <- Retro Split-Flap Clock with NTP TimeSync
│   ├── TouchCalibration.bin   <- Precision Touch Calibration Utility
│   ├── WeatherStation2.8.bin  <- Full Weather Station (2.8" CYD)
│   └── WeatherStation3.5.bin  <- High-Resolution Weather Station (3.5" CYD)
├── BMP32X32/                  <- Compact Weather Icons (Root)
├── BMP64x64/                  <- High-Res Weather Icons (Root)
├── flags/                     <- Country Flags for World Clock (Root)
└── Logo/                      <- AMK System & Splash Logos (Root)
```

### 📦 Included Applications:
1. **✈️ FlightRadar.bin** — Real-time ADS-B aircraft flight tracking on OpenStreetMap with touch pan/zoom.
2. **🕰️ FlipClock.bin** — Retro split-flap clock with automatic WiFi NTP synchronization and multi-city timezone support.
3. **🎯 TouchCalibration.bin** — Interactive 5-point hardware touch screen calibration utility for resistive screens.
4. **☀️ WeatherStation2.8.bin** — Full-featured weather station with current conditions, 7-day forecast, hourly graph, and astronomical sun/moon tracking (compiled for 2.8" CYD).
5. **🌤️ WeatherStation3.5.bin** — High-resolution weather station optimized for 3.5" (320x480) CYD displays.

> ⚠️ **Crucial Asset Placement Rule:**
> - Application `.bin` files **must** go inside the `/apps/` directory.
> - Supporting asset folders like `BMP32X32/`, `BMP64x64/`, and `flags/` **must be placed directly in the ROOT of the SD card**, NOT inside `/apps/`.
>
> 📁 **Download Ready-to-Run Starter Kit:** [AMK_CYD_SD_Card_Files.zip](AMK_CYD_SD_Card_Files.zip) *(Direct Download from this Repository)*


### Example `info.json`:
```json
{
  "name": "Flight Radar Universal",
  "version": "v1.4",
  "author": "Aung Moe Khine",
  "desc": "Live ADS-B Flight Tracking with OpenStreetMap",
  "target": "Universal CYD"
}
```

---

## 🐠 Digital Aquarium

Because Aung Moe Khine loves fish, this web installer embeds an interactive virtual aquarium right below the flashing console! Feed the fish, tap the water to create ripples, and enjoy the underwater ambiance while flashing your hardware.
