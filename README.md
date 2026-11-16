# AMK Multi-Boot App Launcher for ESP32 CYD

A powerful, hardware-level multi-boot application launcher for ESP32 Cheap Yellow Displays (CYD) and smart display modules. Flash the launcher once to your board, then load and switch between any Arduino or ESP-IDF `.bin` applications directly from your MicroSD card.

> **Developer:** Aung Moe Khine  
> **Display Library:** `AMK_CYD_Universal_TFT` (Hardware auto-detection & frozen color standard)

---

## 🌟 Key Features

- **Hardware-Level Dual-Boot:** Boots into the launcher on every startup. Double-tap the screen anytime during the 5-second countdown to switch apps, or do nothing to auto-boot your active application.
- **MicroSD Card Carousel:** Simply drop compiled `.bin` sketches into the `/apps/` folder on a FAT32 MicroSD card — no recompiling or USB reflashing required.
- **Wireless Wi-Fi Web Drop:** Upload new applications directly from your phone or PC browser over local AP (`AMK-Launcher-WiFi`) with instant SD rescan and auto-return countdown modal.
- **Audio DAC Sound FX:** Crisp boot chime, button tap ticks, and action beeps via ESP32 DAC GPIO 26.
- **Universal Hardware Support:** Powered by `AMK_CYD_Universal_TFT`, supporting all ESP32 & ESP32-S3 boards (2.4", 2.8", 3.2", 3.5", 4.0", 4.3", 5.0", and 7.0") with both **Resistive (XPT2046)** and **Capacitive (GT911, CST820, FT6336U)** touch, and USB-C (ESP32-32E) & Micro-USB variants.
- **Light Sensor (LDR) Auto-Dimming:** Dynamic moving-average ambient light monitoring that smooths brightness automatically.
- **Smart Sleep:** Powers down the display backlight after 2 minutes of inactivity; simply tap the screen to wake.
- **Emergency Recovery:** Hold down the **BOOT button (GPIO 0)** during power-on or reset to immediately force-boot back to the launcher menu.

---

## 🆕 What's New in v2.0

- 🚀 **1.21 MB Launcher Partition (`test` @ `0x010000`):** Expanded launcher flash partition to 1,216 KB, providing abundant space for high-fidelity graphics, Wi-Fi web stack, and audio engine without bootloader overflow.
- 📡 **Built-in Wi-Fi Web Drop:** Wirelessly upload `.bin` firmware straight to your MicroSD card via softAP (`AMK-Launcher-WiFi` at `192.168.4.1`) from any smartphone or laptop with a dedicated `Finish & Return` button.
- ⏱️ **Auto-Rescan & Countdown Modal:** Automatic directory scanning immediately after Wi-Fi upload, featuring an animated 3-second countdown modal with tap-to-skip.
- 🔊 **Integrated Audio DAC Feedback:** Natural audio responses via ESP32 DAC on GPIO 26 — boot chime, touch clicks, and success tones.
- 🎨 **Seamless Borderless Boot Splash:** Refined boot screen geometry and background matching, eliminating square logo borders while maintaining 100% natural, crisp 16-bit RGB565 color reproduction.
- 🗺️ **Balanced 1.08 MB SPIFFS Partition:** Dedicated SPIFFS storage (`0x2F0000`) ensuring full compatibility with FlightRadar (OpenStreetMap flight tile caching) and yoRadio (Web UI, playlists, and station logos).

---

## 📁 MicroSD Card Organization & Included Apps

Format your MicroSD card with **FAT32**. Extract the starter kit `AMK_CYD_SD_Card_Files.zip` directly to the root of your MicroSD card:

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
1. **✈️ FlightRadar.bin** — Real-time ADS-B aircraft flight tracking on OpenStreetMap with interactive pan & zoom.
2. **🕰️ FlipClock.bin** — Retro split-flap clock with automatic WiFi NTP time synchronization and multi-city timezone support.
3. **🎯 TouchCalibration.bin** — Interactive touchscreen hardware calibration & test utility with jitter filter tuning, live drawing test, and permanent NVS flash storage.
4. **☀️ WeatherStation2.8.bin** — Full-featured weather station with current conditions, 7-day forecast, hourly temperature graph, and astronomical sun/moon tracking (for 2.8" CYD).
5. **🌤️ WeatherStation3.5.bin** — High-resolution weather station optimized for 3.5" (320x480) CYD displays.

> ⚠️ **Crucial Asset Placement Rule:**
> - Application `.bin` files **must** go inside the `/apps/` directory.
> - Supporting asset folders like `BMP32X32/`, `BMP64x64/`, and `flags/` **must be placed directly in the ROOT of the SD card**, NOT inside `/apps/`.
>
> 📁 **Download Ready-to-Run Starter Kit:** [AMK_CYD_SD_Card_Files.zip](AMK_CYD_SD_Card_Files.zip) *(Direct Download from this Repository)*

---

## 🛠️ Flash Memory Partition Table Layout (4MB Flash)

The installer configures an optimized 4MB flash layout with a dedicated 1.08 MB SPIFFS filesystem:

| Name | Type | SubType | Flash Offset | Size | Purpose |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Bootloader** | `bootloader` | — | `0x001000` | 28 KB | KeyBoot interceptor bootloader |
| **Partitions** | `partition` | — | `0x008000` | 3 KB | AMK Custom Partition Table |
| **otadata** | `data` | `ota` | `0x00E000` | 8 KB | OTA boot partition selector |
| **test** | `app` | `test` | `0x010000` | 1,216 KB | AMK Multi-Boot Launcher Application |
| **app0** | `app` | `ota_0` | `0x140000` | 1,728 KB | Target user application slot (flashed from SD card) |
| **spiffs** | `data` | `spiffs` | `0x2F0000` | 1,088 KB | Dedicated SPIFFS storage for OSM maps (FlightRadar), web UI & assets (yoRadio) |
