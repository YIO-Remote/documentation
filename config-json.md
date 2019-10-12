# YIO config.json dokumentation

## About config.json
The config.json file is used by the YIO software to determine the integrations, the look and use of YIO.  
In this file all data is stored from YIO, so if you like to make a backup of your data, you only need to save this File.  
The following things can be defined in YIOs config file:
- [Areas](https://github.com/YIO-Remote/documentation/blob/master/config-json.md#areas-section)  
- [Entities](https://github.com/YIO-Remote/documentation/blob/master/config-json.md#entities-section)  
- [Integrations](https://github.com/YIO-Remote/documentation/blob/master/config-json.md#integration-section)  
- [Settings](https://github.com/YIO-Remote/documentation/blob/master/config-json.md#settings-section)  
- [UI_Config](https://github.com/YIO-Remote/documentation/blob/master/config-json.md#ui_config-section)
- [Remote](https://github.com/YIO-Remote/documentation/blob/master/config-json.md#remote-section)

## Areas Section
At the Area Section you define what areas you would like to have available at YIOs GUI. YIO is not limiting you to define areas.  

Following Code will be used to determine an Area:
```
"areas": [
        {"area": "Living Room"},
        {"area": "Garden"}
    ],
```

## Entities Section
At the Entitie Section you define what devices will be available at the YIO GUI. YIO is not limiting you to define areas.  

**area Tag:** needs to match a previously defined area (case Senitive)  
**friendly_name Tag:** is user defined and will be used for display the name of the entitie at the YIO UI.  
**integration Tag:** used to determine the integration the interaction with this device will be forwarded to (homeassistant, homey)  

Following Code will be used to determine an Entitie:
```
"entities": {
        "light": [
            {
                "area": "Living Room",
                "entity_id": "light.livingroom",
                "favorite": false,
                "friendly_name": "Main Lamp",
                "integration": "homeassistant",
                "supported_features": [
                    "BRIGHTNESS","COLOR"
                ]
            },
            ,
        "blind": [
            {
                "area": "Living Room",
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
                "area": "Living Room",
                "attributes": {
                    "state": "off"
                },
                "entity_id": "media_player.livingroom",
                "favorite": false,
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

**ip Tag:** needs to point to your integrations server ip and port or to the hostname with port  
**token Tag:** generate a [Home Assistant Long-lived access token](https://developers.home-assistant.io/docs/en/auth_api.html#long-lived-access-token) and copy it here  
**friendly_name Tag:** user defined name will be used for display the name of the integration at the YIO UI settings page.  

Following Code will be used to determine an **home assitant** Integration:
```
"integration": [
        {
            "connectionOpen": true,
            "data": {
                "ip": "XXX.XXX.XXX.XXX:8123",
                "token": "[Long-lived access token](https://developers.home-assistant.io/docs/en/auth_api.html#long-lived-access-token)"
            },
            "friendly_name": "Home Assistant",
            "id": 1,
            "obj": null,
            "plugin": "homeassistant",
            "type": "homeassistant"
        }
    ],
```
Following Code will be used to determine an **homy** Integration:
```
"integration": [
        {
            "connectionOpen": true,
            "data": {
                "ip": "XXX:XXX:XXX:XXX:8936"
            },
            "friendly_name": "Homey",
            "id": 1,
            "obj": null,
            "plugin": "homey",
            "type": "homey"
        }
    ],
```
## Settings Section
At the Settings Section all settings will be saved when changed via the Settings Page.  
You can also preset the settings here for first bootup.  

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
**profile Tag:** generate profiles with this tag, each profile hold the * *name, pages & favorites * * tag  
    **profile/name Tag:** is user defined and will be used for display the name of the profil at the YIO Profile selector  
    **profile/pages Tag:** define the order and the show of pages at the YIO UI  
    **profile/favorites Tag:** define which devices will be displayed at the favorites page for each profile 
**pages Tag:** basically desing your displayed page here it will hold the * *name, image & group * * tag
    **pages/name Tag:** name of the page that will be displayed in the UI and in the selection menu
    **pages/image Tag:** path to image that will be used as headergrapic (images needs to be available, else it will be blank)
    **pages/groups Tag:** groups that will be available at the defined page (e.g. grouped lights, grouped media player or mixed)  
**groups Tag:** will group devices under a header and will hold the * * name, switch & entitie * * tag  
    **groups/name Tag:** user defined header that will be displayed above each group  
    **groups/switch Tag:** set **true** or **false** to enable or disable a group switch  
    **group/entitie Tag:** will hold all devices that should be displayed in this group

Following Code will be used to determine settings:
```
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