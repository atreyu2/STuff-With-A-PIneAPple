

## Overview

This guide explains **step-by-step** how to use a **WiFi Pineapple Mark VII** for wireless testing and MITM-style attacks in simple language. Everything includes **why** we do it and the **realistic limitations** in 2026.

Modern devices (phones, laptops) are much harder to trick than older ones. Success is **not guaranteed** and depends on many conditions.

---

## Table of Contents
- [Initial Setup](#initial-setup)
- [Reconnaissance](#reconnaissance)
- [PineAP Configuration](#pineap-configuration)
- [Rogue AP / Evil Twin](#rogue-ap--evil-twin)
- [Internet Forwarding (Critical for MITM)](#internet-forwarding-critical-for-mitm)
- [Advanced Techniques](#advanced-techniques)
- [Full Realistic Workflow](#full-realistic-workflow)
- [Common Problems & Fixes](#common-problems--fixes)
- [Important Limitations](#important-limitations)

---

## Initial Setup

1. Plug in the Pineapple (USB-C). Attach antennas **before** powering on.
2. Connect your laptop to the Pineapple’s default WiFi (check sticker).
3. Open browser → `http://172.16.42.1:1471`
4. Complete Stager: Connect to internet (phone hotspot works well), update firmware, create admin account.
5. Update **all modules** in the Pineapple Bar.

---

## Reconnaissance

- Go to **Recon** tab.
- Select interface(s) and scan.
- Look for:
  - Nearby access points
  - Connected clients
  - **Probe requests** (devices looking for known networks)

**Why?** Probe requests help you understand what networks devices might connect to.

---

## PineAP Configuration (Core Tool)

PineAP controls rogue access points and impersonation.

### Key Settings:
- Enable **PineAP Daemon**
- Set to **Active** mode
- Enable **Capture SSIDs from Probes**
- Enable **Broadcast SSID Pool**

### KARMA-Style Attacks
- You can impersonate networks from the SSID pool.
- The Pineapple replies to probe requests.

**Reality check**: Modern phones (iOS/Android) use MAC randomization and are cautious about auto-joining open networks. Blind auto-connection is **much rarer** than in the past.

---

## Rogue AP / Evil Twin

**Broad Attack (KARMA-like)**:
- Add common or captured SSIDs to the pool.
- Enable impersonation.

**Targeted Evil Twin**:
- Clone a specific real SSID.
- Make your signal stronger (get closer or use better antennas).

**Important Condition**: The target device must be willing to connect. It usually needs to:
- Be actively probing for that saved network, **or**
- Be manually joined by the user, **or**
- Be forced via deauthentication.

Just broadcasting the SSID is **not magic**.

---

## Internet Forwarding (Critical for MITM)

For real MITM, traffic must flow through the Pineapple to the internet.

### Steps:
1. Give the Pineapple internet access (Ethernet, USB tether, or second radio in client mode).
2. In **Advanced → Networking** (or similar), enable **IP Forwarding** and **NAT/Masquerading**.
3. Ensure the rogue AP interface is correctly bridged/routed to the uplink interface.
4. Check DNS settings so victims can resolve domains.

**Common Failure Point**: Beginners often mess up routing, NAT, or interface selection. Victims connect but get “no internet.” Test thoroughly with your own device first.

Traffic flow should be: **Victim → Pineapple (Rogue AP) → Pineapple Uplink → Real Internet**

---

## Advanced Techniques

### 1. Evil Portal (Captive Portal)
- Install **Evil Portal** module.
- Use templates (e.g., from kleo/evilportals on GitHub).
- The portal can redirect users to a fake login page.

**Reality**: 
- It does **not** automatically steal passwords from normal browsing.
- Success requires the **user to manually enter credentials** into your fake page.
- Many users will notice suspicious behavior and close it.
- HTTPS + HSTS + certificate pinning make passive credential theft very difficult.

### 2. DNS Spoofing
- Install **DNSspoof**.
- Redirect specific domains to your fake pages.
- Works better when combined with Evil Portal, but DNS caching can reduce effectiveness.

### 3. Deauthentication Attacks
- From Recon or Deauth module: Send deauth frames to disconnect clients from real APs.
- Goal: Force reconnection to your rogue AP.

**Reality Check**:
- Deauth is **not guaranteed**.
- Modern devices may quickly reconnect to the real AP, switch to mobile data, show warnings, or ignore the network.
- Management Frame Protection (MFP) on some networks blocks deauth.
- It helps, but it is **not** a magic button.

### 4. Traffic Inspection
- Use built-in captures or export to Wireshark.
- Advanced users can route through mitmproxy or Burp Suite (requires extra setup and often installing certificates on the target device).

---

## Full Realistic Workflow (Lab Only!)

1. Setup + updates complete.
2. Recon to understand the environment.
3. Configure PineAP rogue AP (match target SSID if possible).
4. Set up proper internet forwarding/NAT.
5. (Optional) Run deauth to encourage reconnection.
6. Start Evil Portal + DNS spoof if desired.
7. Monitor connected clients and logs.
8. Be ready for low success rate on modern devices.

---

## Common Problems & Fixes

| Problem                        | Possible Fix |
|--------------------------------|--------------|
| Devices won't connect          | Stronger signal, manual join, better SSID match, deauth |
| No internet for victims        | Fix IP forwarding, NAT, DNS, and uplink interface |
| Portal doesn't appear          | DNS spoof + captive portal detection domains |
| Low success on modern phones   | Test on older devices; combine multiple techniques |
| Deauth doesn't force join      | Devices may prefer real AP or mobile data |

---

## Important Limitations (Read This!)

- **Modern devices resist these attacks** heavily (MAC randomization, auto-connect protections, HTTPS Everywhere, HSTS, certificate pinning).
- Auto-connection is **overstated** — it rarely happens blindly anymore.
- Credential capture via Evil Portal requires **user interaction** and is not automatic.
- Deauth helps but does not guarantee the device will join your network.
- Success rate is much higher in controlled lab environments with cooperative/old devices.
- Signal strength, proximity, and timing matter a lot.

This tool is best used for **authorized security testing** and learning wireless security concepts.

---
