---
title: "Overview"
weight: 1
description: "What Meshtastic is, how LoRa mesh works, and what you can do with it."
---

Meshtastic is an open-source, off-grid text network built on **LoRa** radios. Devices (“nodes”) relay small messages hop-by-hop, so a handful of nodes can cover surprising distances without cell or Wi-Fi.

{{< alert color="info" >}}
New here? After this page, go to **[Getting Started](/docs/getting-started/)** to pick hardware and set up US-915 regional settings for El Paso.
{{< /alert >}}

## What you can do
- Send short texts and GPS waypoints without the internet  
- Coordinate hikes, events, emergency check-ins, or neighborhood comms  
- Experiment with radios, antennas, batteries, and mapping coverage

## How it works (quick)
1. Each node transmits small packets on a regional LoRa band (US-915 in our area).  
2. Nearby nodes repeat those packets, forming a **mesh**.  
3. Your phone connects to your node via Bluetooth/Wi-Fi using the Meshtastic app.  
4. Messages propagate across multiple hops until they reach the destination.

## Hardware at a glance
- **Common boards:** LilyGO T-Beam, Heltec V3, LILYGO T-Deck, etc.  
- **Antenna:** 915 MHz tuned antenna (US). Height and line-of-sight help a lot.  
- **Power:** USB power bank or LiPo; for fixed nodes, consider solar + charge controller.

## Apps & interfaces
- **Mobile:** Android / iOS apps for everyday use  
- **Desktop/Web:** Web client and CLI for power users  
- **Integrations:** Position beacons, MQTT bridges, and mapping tools

## Channels & privacy basics
- Nodes join a **channel** (shared key + settings).  
- Public/demo channels are good for onboarding and range testing.  
- For private groups, use unique keys and share them only with trusted members.

## Range, terrain, and antennas
- Range depends on elevation, antenna quality, interference, and line-of-sight.  
- Urban, ground-level tests may be ~1–3 mi; hill-to-hill or elevated nodes can go much farther.  
- Simple upgrades (better placement/height, tuned antennas) often beat raw transmit power.

## Local etiquette
- Keep messages short and relevant; avoid spamming.  
- Identify yourself when testing and note intended duration/power changes.  
- Be courteous with shared/public channels; move longer chats to private channels.

## Next steps
- **Set up your first node:** head to **[Getting Started](/docs/getting-started/)**  
- **Join the community:** hop into our **Discord** from the header for channel details and meetups  
- **See coverage tools:** check **[Map & Data](/map/)** for Explorer/Meshview links and mapping notes
