# WiFi Pineapple Wireless Security Lab

## Overview

This repository documents a controlled WiFi Pineapple wireless security lab focused on understanding rogue access point risks, wireless client behavior, and defensive awareness.

The purpose of this project is to study wireless security concepts in a legal lab environment and explain the risks that organizations should understand when users connect to untrusted wireless networks.

## Ethics and Legal Disclaimer

This project is for cybersecurity education only.

All testing should be performed only in a controlled lab environment using devices, networks, and accounts that you own or have explicit permission to test.

This repository is not intended to support unauthorized access, credential theft, public network attacks, or testing against third-party systems.

## Lab Goals

The goals of this project are to better understand:

- Rogue access point concepts
- Wireless client trust behavior
- Evil twin and impersonation risks at a high level
- Wireless network awareness
- Defensive detection and prevention ideas
- How users can be tricked into joining unsafe wireless networks
- Why secure wireless configuration matters

## Tools and Concepts

| Area | Purpose |
|---|---|
| WiFi Pineapple | Wireless security testing platform used in a controlled lab |
| Test wireless clients | Devices used to observe safe lab behavior |
| Lab SSIDs | Controlled wireless networks created for testing |
| Wireless security controls | Defensive ideas such as trusted SSIDs, user awareness, and monitoring |
| Documentation | Notes, screenshots, and lessons learned from the lab |

## What This Project Demonstrates

This project is meant to show practical understanding of wireless security risks, including:

- Why users should avoid unknown or suspicious WiFi networks
- How rogue access points can create security risk
- Why wireless monitoring and user awareness matter
- How attackers may abuse trust in familiar network names
- Why organizations should educate users on wireless safety
- How defensive teams can think about detection and prevention

## Defensive Takeaways

Important defensive lessons from this type of lab include:

- Avoid automatically joining open or unknown WiFi networks
- Disable auto-join for public networks when possible
- Use VPNs on untrusted networks when appropriate
- Monitor for suspicious or duplicate SSIDs in business environments
- Train users to recognize wireless risks
- Use enterprise wireless security controls where possible
- Treat wireless access as part of the security perimeter

## Repository Structure

```text
.
├── README.md
├── docs/
│   ├── setup-notes.md
│   ├── testing-notes.md
│   └── lessons-learned.md
└── screenshots/
```

## Suggested Screenshots

Add screenshots to a `screenshots/` folder using clean file names such as:

```text
wifi-pineapple-dashboard.png
lab-ssid-configuration.png
wireless-client-test.png
detection-notes.png
```

## Portfolio Summary

This lab helps demonstrate hands-on interest in wireless security, responsible security testing, documentation, and defensive thinking.

The main value of the project is not just using a tool. The value is understanding what risk the tool represents, how the attack surface works, and how defenders can reduce that risk.

## Disclaimer

This is a homelab project for learning and documentation. It should only be used in authorized environments.
