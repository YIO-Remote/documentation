# YIO config.json dokumentation

## About config.json
The config.json file is used by the YIOSoftware to determine the Integrations, the look and use of YIO. In this file all data is stored from YIO, so if you like to make a Backup of your data, you only need to save this File.  
The following things can be defined in YIOs config file:
- [Areas](https://github.com/YIO-Remote/documentation/blob/master/config-json.md#areas-section)  
- [Entities](https://github.com/YIO-Remote/documentation/blob/master/config-json.md#entities-section)  
- [Integrations](https://github.com/YIO-Remote/documentation/blob/master/config-json.md#integration-section)  
- [Settings]
- [UI_Config]
- [Remote]

## Areas Section
At the Area Section you define what Areas you would like to have available at YIOs GUI. There is no limit for area entries defined.  

Following Code will be used to determine an Area:
```
"areas": [
        {"area": "Living Room"},
        {"area": "Garden"}
    ],
```

## Entities Section
At the Entitie Section you define what devices will be available at the YIO GUI.There is no limit for area entries defined.  
**area Tag:** needs to match a previously defined Area (case Senitive)  
**friendly_name Tag:** is user defined and will be used for display the Name at the YIO UI.  

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

**ip Tag:** needs to point to your Home Assistant Server IP or to the hostname  
**token Tag:** Generate a HA [Long-lived access token](https://developers.home-assistant.io/docs/en/auth_api.html#long-lived-access-token) and copy it here  
**friendly_name Tag:** Device a name for your Integration  

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