---
title: "Homey"
description: "Home automation software Homey for which an integration has been written."
menu:
  docs:
    parent: "integrations"
weight: 20
---

Home automation software ([website](https://www.athom.com/en/homey/)) for which an integration has been written. This enables certain devices exposed by Homey to be controllable by the YIO remote.

The currently supported devices are:

- [x] Blinds
- [ ] Heating
- [x] Lights
- [ ] Media players
- [ ] PVR / TV

Example config.json file for Homey Integration will be available later.

## How to install

...
The App can be downloaded at https://apps.athom.com/ when it reaches the public beta stage.
Code can be manually installed using athom-cli if desired, code can be found https://github.com/nklerk/nl.nielsdeklerk.yio

## Supported entities

- "blind"
- "light"
- "media_player"

## Configuration

Edit the `config.json` file on YIO and include the homey "IPaddress:8936" like:

```
  "integrations": {
    "homey": {
      "data": [
        {
          "data": {
            "ip": "10.2.1.8:8936"
          },
          "friendly_name": "Homey Huiskamer",
          "id": "homey1"
        },
        {
          "data": {
            "ip": "10.2.1.34:8936"
          },
          "friendly_name": "Homey Pro",
          "id": "homey2"
        }
      ],
      "mdns": "_yio2homeyapi._tcp"
    }
  }
```

Edit the `config.json` file on YIO and include the devices you want to controll like:

```
"light": [
      {
        "area": "Huiskamer",
        "entity_id": "9831cb7a-b2ef-4d37-a729-f7768b93a1a5",
        "friendly_name": "TV LEDStrip",
        "integration": "homey1",
        "supported_features": [
          "BRIGHTNESS",
          "COLORTEMP",
          "COLOR"
        ]
      }
  ]
```

While starting the app using "athom app run" the app start's printing all compatible devices in the yio format for easy adding.

## Homey App - YIO Remote communication

YIO Remote connects to the Homey app via websockets. The following commands are sent on connection.

When the connection is established, Homey app sends the following JSON data to confirm the connection.

```
      {
            "type": "connected"
      }
```

Homey app ask the remote to send over the configured entities with the command:

```
      {
            "type": "command",
            "command": "get_config"
      }
```

YIO Remote gets the entities from the configuration file and sends over a list of entity ids to the Homey app:

```
      {
            "type": "sendConfig",
            "devices":[
                  "a2874619-f558-0987-af71-099860739bb2",
                  "a012c098-f448-4273-a074-930660739cc1"
            ]
      }
```

After receiving the entity ids, Homey App sends the current states and properties of each device:

```
      {
            "type": "command",
            "command": "send_states",
            "data": {
                  "entity_id": "a012c098-f448-4273-a074-930660739cc1",
                  "onoff": "on",
                  "friendly_name": "Mooie lamp",
                  "supported_features": ["BRIGHTNESS"]
            }
      }
```

When an event occurs in Homey, the Homey app sends an update to the remote:

```
      {
            "type":"event",
            "data":{
                  "entity_id":"a012c188-f448-4273-a074-930660739cc1",
                  "dim":0.09
            }
      }
```

When YIO is used to command a state change the YIO send the folowing to the Homey app.

```
      {
            "type": "command",
            "command": "onoff",
            "deviceId": "a012c098-f448-4273-a074-930660739cc1",
            "value": true
      }
```
