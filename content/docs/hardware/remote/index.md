---
title: "Remote"
description: "The YIO remotes is designed as HID for all kind of SmartHome backend Systems and SmartHome devices."
menu:
  docs:
    parent: "hardware"
weight: 20
---

The YIO remotes is designed as [HID](https://en.wikipedia.org/wiki/Human_interface_device) for all kind of SmartHome backend Systems and SmartHome devices. More Information about supported Integrations can be found in the menu.

### YIO Remote Hardware

- based on a **Raspberry Pi Zero W** (used as mainboard and provides connections via Wi-Fi and Bluetooth)
- **13 customizable physical buttons** (2 different functions at each button can be assigned)
- **3.5" high resolution capacitive touchsreen** (480x800px resolution)
- **ambient light sensor** (detects you light scenario and dims / brightens the display)
- **battery monitoring** (charge, health & voltage)
- **custom designed housing** (CNC milled, sandblasted and anodized)
- **gesture sensor** (detects you and switches on the display)
- **haptic feedback motor** (gives vibrating feedback)
- **proximity sensor** (detects proximity and switches on the display)

### YIO Remote Software

The Project is based on a custom [buildroot](https://buildroot.org/) system with UI designed and programmed in [Qt Open Source](https://www.qt.io/download-open-source). Remote and Dock communicate with each other to enable IR Support bidirectional.
