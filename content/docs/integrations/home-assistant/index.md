---
title: "Home Assistant"
description: "Original SmartHome Backend software Home Assistant for which an integration has been written."
menu:
  docs:
    parent: "integrations"
weight: 10
---

Original SmartHome Backend software ([website](https://www.home-assistant.io/)) for which an integration has been written. This enables certain devices exposed by Home Assistant to be controllable by the YIO remote.

The currently supported devices are:

- [x] Blinds
- [x] Heating
- [x] Lights
- [x] Media players
- [ ] PVR / TV

## UI configuration

Open the web configurator, navigate to integrations and add a new Home Assistant instance.

## Manual configuration

Add this to the `integrations` section of the config.json to use the Home Assistant integration:

```jsonc
"integrations": {
    "homeassistant": {
        "data": [
            {
                "data": {
                    "ip": "192.168.1.XXX:8123 or example.com", // Your HA IP address or hostname
                    "token": "eyJ0xxxxxx", // Long-lived access token
                    "ssl": "true", // Use https/wss (optional)
                    "ignoreSsl": "true" // Ignore SSL errors (optional)
                },
                "friendly_name": "Home Assistant", // Friendly Name shown in integrations menu
                "id": "homeassistant" // Must be unique
            }
        ],
        "mdns": "_hap._tcp"
    }
}
```

To add more Home Asssistant instances, add additional objects in the data array like so:

```jsonc
"integrations": {
    "homeassistant": {
        "data": [
            {
                ...
                "id": "homeassistant"
            },
            {
                ...
                "id": "homeassistant2"
            }
        ],
        "mdns": "_hap._tcp"
    }
}
```

### Data

- **ip**: IP Address or hostname to connect to the Home Assistant instance
- **token**: Generate a [Home Assistant Long-lived access token](https://developers.home-assistant.io/docs/en/auth_api.html#long-lived-access-token) and copy it here
- **ssl**: Uses HTTPS/WSS to connect to Home Assistant
- **ignoreSsl**: Ignores any SSL errors, necesssary for self-signed certificates

### Misc.

- **friendly_name**: Name shown in the integrations configuration page
- **id**: Unique ID used to identify this specific integration

## Entity example

To add entities, use the entity_id specified in the Home Assistant instance. Example with a light entity:

```json
"entities": {
    "light": [
        {
            "area": "Example Area",
            "entity_id": "light.examples_light_1",
            "friendly_name": "Example Light 1",
            "integration": "homeassistant",
            "supported_features": [
                "BRIGHTNESS",
                "COLORTEMP",
                "COLOR"
            ]
        }
    ]
},
```
