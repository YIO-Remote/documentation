---
title: "OpenHAB"
description: "Home automation software OpenHAB for which an integration has been written."
menu:
  docs:
    parent: "integrations"
weight: 30
---

Home automation software ([website](https://www.openhab.org/)) for which an integration has been written. This enables certain devices exposed by OpenHAB to be controllable by the YIO remote.

The currently supported devices are:

- [x] Blinds
- [ ] Heating
- [x] Lights
- [x] Media players
- [ ] PVR / TV

Sample config.json **Integration entry** :

        "openhab":
        {
            "log": "debug",
            "data":
            [
                {
                    "url": "http://192.168.1.100:8080/rest",
                    "polling_interval": 2000,
                    "friendly_name": "RIC's OpenHab",
                    "id": "openhab"
                }
            ]
        },

For **normal** entities simply use the **item name** (not the label) as **entity_id**.

For **media player** entities use the things UID (for example "`fsinternetradio:radio:bcd00a3c`") as **entity_id** and link the things cannels to items.
