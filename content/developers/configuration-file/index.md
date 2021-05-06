---
title: "Configuration file"
description: "The config.json file is used by the YIO software to determine the integrations, the look and use of YIO."
menu:
  developers:
    parent: "home"
weight: 40
---

## About config.json

The config.json file is used by the YIO software to determine the integrations, the look and use of YIO.  
All data is stored from YIO in this file. To backup the YIO data, only the config.json file has to be backed up.  
The following things can be defined in YIOs config file:

- [Entities](developer-config-json#entities-section)
- [Integrations](developer-config-json#integration-section)
- [Settings](developer-config-json#settings-section)
- [UI_Config](developer-config-json#ui_config-section)
- [Remote](developer-config-json#remote-section)

## Entities Section

At the Entity Section you define what devices will be available at the YIO GUI. YIO is not limiting you to define areas.

**friendly_name**: Is used for displaying the entity's name at the YIO UI.
**integration**: Determines the integration used for the entity (e.g. homeassistant, homey, ...)

Following code will be used to determine an entity:

```json
"entities": {
        "light": [
            {
                "entity_id": "light.livingroom",
                "friendly_name": "Main Lamp",
                "integration": "homeassistant",
                "supported_features": [
                    "BRIGHTNESS","COLOR"
                ]
            },
            ,
        "blind": [
            {
                "entity_id": "cover.living_room_blinds_level",
                "friendly_name": "Living room blinds",
                "integration": "homeassistant",
                "supported_features": [
                    "OPEN",
                    "CLOSE",
                    "STOP",
                    "POSITION"
                ]
            },
         "media_player": [
            {
                "entity_id": "media_player.livingroom",
                "friendly_name": "Living Room Sonos",
                "integration": "homeassistant",
                "supported_features": [
                    "SOURCE",
                    "APP_NAME",
                    "VOLUME",
                    "VOLUME_UP",
                    "VOLUME_DOWN",
                    "VOLUME_SET",
                    "MUTE",
                    "MUTE_SET",
                    "MEDIA_TYPE",
                    "MEDIA_TITLE",
                    "MEDIA_ARTIST",
                    "MEDIA_ALBUM",
                    "MEDIA_DURATION",
                    "MEDIA_POSITION",
                    "MEDIA_IMAGE",
                    "PLAY",
                    "PAUSE",
                    "STOP",
                    "PREVIOUS",
                    "NEXT",
                    "SEEK",
                    "SHUFFLE",
                    "TURN_ON",
                    "TURN_OFF"
                ]
            },
        ]
    },
```

## Integration Section

At the Integration Section you define what integration is available for YIO.  
Following Integrations are actually possible:

[x] Home Assistant
[x] Homey
[ ] openHAB

- **ip**: IP Address or hostname to connect to the Home Assistant instance
- **token**: Generate a [Home Assistant Long-lived access token](https://developers.home-assistant.io/docs/en/auth_api.html#long-lived-access-token) and copy it here
- **ssl**: Uses HTTPS/WSS to connect to Home Assistant
- **ignoreSsl**: Ignores any SSL errors, necesssary for self-signed certificates
- **friendly_name**: User defined name will be used for display the name of the integration at the YIO UI settings page.
- **id**: Unique ID used to identify this specific integration

Following Code will be used to determine an **Home Assistant** Integration:

```jsonc
"integrations": {
    "homeassistant": {
        "data": [
            {
                "data": {
                    "ip": "XXX.XXX.XXX.XXX:8123 or example.com",
                    "token": "Long-lived access token",
                    "ssl": "true", // (optional)
                    "ignoreSsl": "true" // (optional)
                },
                "friendly_name": "Home Assistant",
                "id": "homeassistant"
            }
        ],
        "mdns": "_hap._tcp"
    }
}
```

Following Code will be used to determine an **Homey** Integration:

```json
"integrations": {
    "homey": {
        "data": [
            {
                "data": {
                    "ip": "XXX.XXX.XXX.XXX:8936"
                },
                "friendly_name": "Homey Pro",
                "id": "homey1"
            }
        ],
        "mdns": "_yio2homeyapi._tcp"
    }
  }
```

## Settings Section

At the Settings Section, all settings will be saved when changed via the Settings Page.  
You can also configure the settings here for first bootup.

**language Tag:** add a predefined language here (needs to be available at YIO, else englisch is used as default)  
**autobrightness Tag:** set **true** or **false** to enable or disable auto brightness of the display  
**proximity Tag:** changes the sensitivity of the proximity sensor (value from 0 to 255)  
**shutdowntime Tag:** time in minutes when the remote shut itself down (value from 0 (endless) to 7200 (8h))  
**softwareupdate Tag:** set **true** or **false** to enable or disable automatic updates to the software  
**wifitime Tag:** time in minutes when the remote shuts down the Wi-Fi connection (value 0 (endless) to 60 (1h))  
**bluetootharea Tag:** set **true** or **false** to enable or disable bluetooth area scanning

Following Code will be used to determine settings:

```
"settings":
    {
        "language": "en_US",
        "autobrightness": true,
        "proximity": 40,
        "shutdowntime": 7200,
        "softwareupdate": false,
        "wifitime": 240,
        "bluetootharea": false
    },
```

## UI_Config Section

At the UI_Config Section the complete UI will be defined. YIO will not limit you where you like your devices to be or how many pages you would like to have.

**selected_profile Tag:** here YIO will save what profile was used last, you can determine it here as well  
**profile Tag:** generate profiles with this tag, each profile hold the _name, pages & favorites_ tag  
 **profile/name Tag:** is user defined and will be used for display the name of the profil at the YIO Profile selector  
 **profile/pages Tag:** define the order and the show of pages at the YIO UI  
 **profile/favorites Tag:** define which devices will be displayed at the favorites page for each profile  
**pages Tag:** basically desing your displayed page here it will hold the _name, image & group_ tag  
 **pages/name Tag:** name of the page that will be displayed in the UI and in the selection menu  
 **pages/image Tag:** path to image that will be used as headergrapic (images needs to be available, else it will be blank)  
 **pages/groups Tag:** groups that will be available at the defined page (e.g. grouped lights, grouped media player or mixed)  
**groups Tag:** will group devices under a header and will hold the _name, switch & entities_ tag  
 **groups/name Tag:** user defined header that will be displayed above each group  
 **groups/switch Tag:** set **true** or **false** to enable or disable a group switch  
 **group/entities Tag:** will hold all devices that should be displayed in this group

Following Code will be used to determine settings:

```json
"ui_config":
    {
        "darkmode": true,
        "selected_profile": 0,
        "profiles": {
            "0": {
                "name": "YIO",
                "pages": ["favorites","0","settings"],
                "favorites": ["light.livingroom"]
            },
            "1": {
                "name": "Standard",
                "pages": [],
                "favorites": ["favorites","1","settings"]
            }
        },
        "pages": {
            "0": {
                "name": "Living Room",
                "image": "/usr/yio-remote/living.png",
                "groups": ["0"]
            },
            "1": {
                "name": "Garden",
                "image": "/usr/yio-remote/garden.png",
                "groups": ["1","2"]
            }
        },
        "groups": {
            "0": {
                "name": "Lights",
                "switch": true,
                "entities": ["light.livingroom", "light.livingroom2", "light.livingroom3"]
            },
            "1": {
                "name": "Lights",
                "switch": true,
                "entities": ["light.garden","light.garden1"]
            },
            "2": {
                "name": "Sonos",
                "switch": false,
                "entities": ["media_player.garden"]
            }
        }
    },
```

## Remote Section

At the Remote Section the IR Devices wil be saved. You are able to define the IR Codes that need to be send here, or you can learn these codes via the YIO UI.

**area Tag:** needs to match a previously defined area (case Senitive)  
**entity_id Tag:** used to determine the entity id that is connected to this IR code  
**friendly_name Tag:** is user defined and will be used for display the name of the IR device  
**integration Tag:** determine the integration the interaction with this remote will be forwarded to (ir, tcp_ip)  
**supported_features Tag:** define features that are supported from the device  
**commands Tag:** define the command that is used to control your device  
 **commands/button_map Tag:** map software or hardware buttons to this command

Following Code will be used to determine a IR remote:

```json
"remote": [
            {
                "area": "Living Room",
                "entity_id": "remote.living_room",
                "friendly_name": "Living Room TV",
                "integration": "ir",
                "supported_features": ["POWER TOGGLE", "CHANNEL UP"],
                "commands": [
                    {
                        "code": "R1,0000,0067,0000,0015,0060,0018,0018,0018,0030,0018,0030,0018,0030,0018,0018,0018,0030,0018,0018,0018,0018,0018,0030,0018,0018,0018,0030,0018,0030,0018,0030,0018,0018,0018,0018,0018,0030,0018,0018,0018,0018,0018,0030,0018,0018,03f6",
                        "button_map": "POWER_TOGGLE"
                    },
                    {
                        "code": "R2,0000,0067,0000,0015,0060,0018,0018,0018,0030,0018,0030,0018,0030,0018,0018,0018,0030,0018,0018,0018,0018,0018,0030,0018,0018,0018,0030,0018,0030,0018,0030,0018,0018,0018,0018,0018,0030,0018,0018,0018,0018,0018,0030,0018,0018,03f6",
                        "button_map": "CHANNEL_UP"
                    }
                ]
            }
        ]
```
