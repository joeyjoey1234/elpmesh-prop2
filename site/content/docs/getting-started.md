---
title: "Getting Started"
weight: 2
description: "Pick hardware, flash Meshtastic, pair the app, and set US-915 for El Paso."
---

This page gets you from zero to your **first working Meshtastic node** in El Paso (US-915).  
If you hit a snag, jump into **Discord** (top right) — we’ll help.

{{< alert color="info" >}}
**Checklist**  
1) Choose hardware • 2) Flash firmware • 3) Pair the app • 4) Set **US-915** • 5) Send a test message • 6) Join our local channel (ask in Discord)
{{< /alert >}}

## 1) Choose hardware (US-915 friendly)
Pick one of these common boards — they’re easy to flash and well supported:

- **LILYGO T-Beam** (built-in GPS, battery header; great starter)
- **Heltec LoRa32 V3** (compact, onboard screen)
- **LILYGO T-Deck / T-Echo / T-Watch** (more UI options)
- **Stationary node** ideas: ESP32 + LoRa module with good external antenna, solar

**Antenna:** use a **915 MHz** antenna. Height and line-of-sight matter more than raw power.

## 2) Flash the Meshtastic firmware
You have two easy paths:

### Option A — Web Flasher (easiest)
- Use the official in-browser flasher (Chrome/Edge recommended).
- Select your board, connect USB, click **Install**.

### Option B — CLI (power users)
- Install `esptool.py` and download the correct `.bin` for your board.
- Put the device in bootloader mode (usually **BOOT** + reset).
- Example command pattern (addresses/filenames vary by board):
  ```bash
  esptool.py --chip esp32 --baud 921600 write_flash -z 0x0 firmware.bin
  ```
- After flashing, power-cycle the device.

> Tip: If the screen shows garbage after flashing, re-flash with the correct build **for your exact board**.

## 3) Pair the app
- Install the **Meshtastic** app (Android/iOS).  
- Power your node, then pair via **Bluetooth** (or Wi-Fi for boards that support it).  
- The app will sync settings and show battery, GPS, and channel info.

## 4) Set the region to **US-915**
- In the app, open **Device → Radio / Region** and select **US-915**.  
- Save/apply, then reboot the node if prompted.

> Using the right region keeps you legal and improves network compatibility.

## 5) Send a first test message
- From the app, send a short message to **All** (or a buddy) and confirm you see **TX/RX** on the device/app.  
- If you’re alone, you’ll still see your node’s own messages — that confirms basic operation.

## 6) Join the El Paso channel
- We share **current channel settings** (PSK / channel name, etc.) in Discord.  
- In the app: **Channels → Add / Import**, then paste or scan what we provide.

> We keep onboarding easy while avoiding publishing private settings on the public site.

---

## Recommended starter settings
- **Position/Telemetry:** on for mapping/range tests (set a sane interval, e.g., 2–5 min)
- **Role:** “Client” for handhelds; “Router/Repeater” for fixed nodes with good elevation
- **Power:** enable screen timeout; for fixed nodes use stable USB or solar
- **GPS:** useful for waypoints/range; disable if you’re optimizing battery and don’t need it

## Antenna & placement quick wins
- Elevate the antenna (even a few meters helps).  
- Keep coax runs short and use low-loss cable if you extend.  
- Avoid placing the antenna right next to large metal surfaces.

## Troubleshooting basics
- **No Bluetooth?** Reboot the node and phone, re-pair.  
- **No GPS lock?** Give it sky view; first lock can take a few minutes.  
- **No messages?** Verify **US-915**, correct channel, and that your friend is in range.  
- **Random reboots?** Try a different USB cable/power source; check battery connections.

---

## Next steps
- **Weekly Net:** see **[Weekly Net](/docs/weekly-net/)** for when/where and what to bring  
- **FAQ:** common questions on range, power, antennas in **[FAQ](/docs/faq/)**  
- **Maps:** live tools in **[Map & Data](/map/)**  
- **Links:** curated resources in **[Useful Links](/links/)**
