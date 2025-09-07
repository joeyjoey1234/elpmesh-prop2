---
title: "FAQ"
weight: 99
description: "Common questions on range, antennas, batteries, privacy, and troubleshooting."
---

{{< alert color="info" >}}
Quick links: **[Map & Data](/map/)** • **[Useful Links](/links/)** • **[Weekly Net](/docs/weekly-net/)**
{{< /alert >}}

### How far does Meshtastic reach?
It depends on **elevation, line‑of‑sight (LOS), antenna quality, interference, and placement**.  
- Urban/ground level: ~1–3 mi is common.  
- Hill‑to‑hill or elevated nodes: much farther.  
Raising the antenna and improving LOS usually beats adding power.

### Do I need a license?
Most Meshtastic defaults in the US use **US‑915** ISM band and **don’t require an amateur radio license**. Always follow your local regulations and stick to the correct **Region** setting.

### Which hardware should I buy first?
Great starters: **LILYGO T‑Beam**, **Heltec LoRa32 V3**, or a **T‑Deck** if you want a keyboard/screen. Any node with **915 MHz** support works for our area.

### What antenna should I use?
Use a **915 MHz‑tuned** antenna. Elevate it when possible, avoid long lossy coax runs, and keep the antenna clear of large metal surfaces. During meetups we compare antennas side‑by‑side.

### What’s a good telemetry (position) interval?
For mapping during tests: **2–5 minutes** is a good baseline. Use **shorter** (e.g., 60–90s) only if the mesh isn’t congested. For battery‑sensitive nodes, lengthen intervals.

### What “role” should my node use?
- **Client:** handheld/portable nodes.  
- **Router/Repeater:** fixed nodes with stable power and good elevation.  
- **Tracker:** mobile nodes that beacon position more frequently.

### How do channels and privacy work?
A **channel** combines a name, PSK (key), and settings. Keep public demo channels for onboarding/range tests, and use **unique PSKs** for private groups. Share keys **only** with trusted members.

### Can I use GPS waypoints?
Yes. GPS helps with **waypoints** and **coverage mapping**. Disable GPS if you don’t need it and want to save battery.

### The app isn’t pairing / I don’t see messages — help!
- Confirm **US‑915** region.  
- Verify you’re on the **same channel** (ask in Discord if unsure).  
- Reboot the node and your phone; re‑pair Bluetooth.  
- Try a different USB cable/power source if the device behaves oddly.  
- Move to a spot with better **LOS** and try again.

### What batteries and power setups are common?
- **Portable:** USB power bank or LiPo (mind charging safety).  
- **Fixed:** stable USB or solar with a **proper charge controller**.  
Enable screen timeout and avoid unnecessary peripherals to stretch runtime.

### How do I contribute to mapping?
See **[Map & Data](/map/)** for live tools and tips. At meetups we run controlled tests and share logs so the community can fill coverage gaps.

### Where do I get help and local channel settings?
Join our **Discord** from the header. Look for **#help** and **#meetups** channels; event threads include temporary channel details and QR imports.

---

Still stuck? Check **[Getting Started](/docs/getting-started/)** and bring the node to a **[Weekly Net](/docs/weekly-net/)**—we’ll get you sorted.
