---
title: "Map & Data (Overview)"
weight: 90
description: "Live tools for viewing nodes and traffic, plus mapping tips for El Paso."
---

This page gives a quick overview of **live map tools** and **mapping best practices**.  
For the full public list we maintain, see **[/map/](/map/)**.

{{< alert color="info" >}}
**Live tools (external):**
- **MeshMap:** <https://meshmap.net/> (Populated by MQTT)
- **Liam Cottle Map:** <https://meshtastic.liamcottle.net> (Populated by MQTT)
- **Meshsense Map:** <https://meshsense.affirmatech.com/> (Populated by MeshSense)
{{< /alert >}}

## Mapping basics
- **Telemetry interval:** 2–5 minutes is a good mapping baseline; go shorter (60–90s) only when the mesh isn’t congested.
- **Consistency:** Change one variable at a time (antenna, height, location) so results are comparable.
- **Elevation & LOS:** Height often matters more than raw power. Try to get above clutter and aim for line-of-sight.
- **Notes:** When testing, add simple notes to messages (e.g., “Test A @ park bench”). Screenshots help later.

## Privacy & hygiene
- **Channel choice:** Use public/demo channels for range tests; use private channels (unique PSK) for day-to-day comms.
- **Location awareness:** If you don’t want to appear on public maps, **disable position beacons** or switch to a private channel.
- **Rate limits:** Don’t spam frequent telemetry if a lot of people are testing at once.
- **After events:** Switch back to your normal channel and adjust telemetry intervals for battery life.

## Data we care about
- **RSSI / SNR** snapshots when you try a new antenna or placement
- **Distance + terrain context** (e.g., hill-to-hill vs. street-level)
- **Failure points** (where packets stop making it through)

## Contribute
- Share your **screenshots or exported tracks** in our Discord event threads.
- Bring your node to the **[Weekly Net](/docs/weekly-net/)** for structured tests.
- If you host a **fixed relay node**, post approximate area and elevation so others can plan paths.

---

See the main **[Map & Data](/map/)** page for the current curated list of viewers and tools.
