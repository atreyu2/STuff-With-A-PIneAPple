## Overview

This guide explains **step-by-step** how to perform MITM attacks using a **WiFi Pineapple Mark VII** in simple 10th-grade language. Everything is explained **why** we do it and what is actually happening.

**Main Goal**: Make devices connect to your Pineapple instead of the real router so you sit "in the middle" of their internet connection.

---

## Table of Contents
- [Initial Setup](#initial-setup)
- [Reconnaissance](#reconnaissance)
- [PineAP Configuration (Core Tool)](#pineap-configuration-core-tool)
- [Creating Rogue AP / Evil Twin](#creating-rogue-ap--evil-twin)
- [Enabling Internet Through Pineapple](#enabling-internet-through-pineapple)
- [Advanced MITM Techniques](#advanced-mitm-techniques)
- [Full Attack Workflow](#full-attack-workflow)
- [Common Problems & Fixes](#common-problems--fixes)
- [Important Limitations](#important-limitations)

---

## Initial Setup

1. Plug in the Pineapple using USB-C cable.
2. On your laptop, connect to the Pineapple’s default WiFi (check the sticker on the device for SSID and password).
3. Open your browser and go to:  
   `http://172.16.42.1:1471`
4. Follow the **Stager** setup:
   - Connect Pineapple to internet (phone hotspot is easiest).
   - Update to the latest firmware (takes 10–20 minutes).
   - Create admin username and password.
5. After reboot, log back into the same address.
6. Go to **Pineapple Bar** (module manager) and update **all** modules.

---

## Reconnaissance

- Go to the **Recon** tab.
- Select your wireless interface (usually `wlan1`).
- Click **Scan**.
- You will see:
  - Nearby real WiFi networks
  - Connected devices (clients)
  - **Probe requests** — phones asking “Is my saved WiFi here?”

**Why?** This helps you know which networks to impersonate.

---

## PineAP Configuration (Core Tool)

PineAP is the most important part of the Pineapple.

### Key Settings:
- Turn **PineAP Daemon** → **ON**
- Set mode to **Active**
- Enable **Capture SSIDs from Probes** (automatically adds networks phones are looking for)
- Enable **Broadcast SSID Pool**

### KARMA Attack (Very Powerful)
- Turn on **Impersonate All Networks**
- The Pineapple will reply “Yes” to every probe request from phones
- Many phones will automatically connect to your fake networks

---

## Creating Rogue AP / Evil Twin

**Method 1: Broad KARMA Attack**
- Add popular SSIDs to the pool (StarbucksWiFi, Free_Public_WiFi, etc.)
- Enable impersonation
- Devices that remember those networks may connect automatically

**Method 2: Targeted Evil Twin**
- Add the **exact same SSID** as the real network you want to attack
- Get closer or use better antennas so your signal is stronger
- (Optional) Use Deauthentication to kick devices off the real network

---

## Enabling Internet Through Pineapple

For the attack to work properly (victims get internet):

1. Connect Pineapple to real internet:
   - Ethernet cable, or
   - USB tether from phone, or
   - Second WiFi radio in Client mode
2. Enable **IP Forwarding / NAT** (usually in Advanced → Networking)
3. Now traffic flow is:  
   **Victim Device → Pineapple → Real Internet**

You are now doing **real MITM**.

---

## Advanced MITM Techniques

### 1. Evil Portal (Captive Portal Phishing)
- Install **Evil Portal** module
- Download templates from GitHub (`kleo/evilportals`)
- Choose a fake login page (WiFi login, Facebook, etc.)
- When someone connects → they see your fake login page
- You capture their username and password

### 2. DNS Spoofing
- Install **DNSspoof** module
- Redirect websites (example: facebook.com → your fake page)
- Combine with Evil Portal for better results

### 3. Deauthentication Attacks
- Select a device in Recon
- Send deauth packets
- Forces the device to disconnect from real WiFi and hopefully join yours

### 4. Traffic Capture
- Use built-in packet capture
- Export to Wireshark
- Advanced: Route through Burp Suite or mitmproxy

---

## Full Attack Workflow (Lab Only!)

1. Complete initial setup and updates
2. Run Recon scan
3. Configure PineAP (add target SSID, enable Active + Impersonate)
4. (Optional) Deauth real network
5. Start Evil Portal + DNS Spoof
6. Monitor **Clients** tab
7. Check logs for captured data

---

## Common Problems & Fixes

| Problem                    | Fix |
|---------------------------|-----|
| Devices won't connect     | Use better antennas, get closer, try deauth |
| Victims have no internet  | Check IP Forwarding / NAT settings |
| Modern phones ignore it   | Works better on older devices |
| Pineapple not responding  | Restart device, check firmware |

---

## Important Limitations

- Modern phones use randomized MAC addresses and are more careful with auto-connect
- HTTPS and HSTS make password stealing harder
- Works best on misconfigured or older devices
- Signal strength is very important

---

## Final Reminder

**Only use this ethically on networks you own.**  
This guide is for educational purposes only.

---

**Made for learning**  
Last Updated: June 2026
