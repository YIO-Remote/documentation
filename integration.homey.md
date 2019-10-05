
## How to install

...
The App can be downloaded at https://apps.athom.com/ when it reaches the public beta stage.
Code can be manually installed using athom-cli if desired, code can be found https://github.com/nklerk/nl.nielsdeklerk.yio

## Supported entities

- light

## Configuration

Edit the `config.json` file on YIO and include the homey "IPaddress:8936" like:

```
"integration": [{
    "data": {
        "ip": "10.0.0.5:8936"
    },
    "friendly_name": "Homey",
    "id": 1,
    "obj": null,
    "plugin": "homey",
    "type": "homey"
    }
],
```

Edit the `config.json` file on YIO and include the devices you want to controll like:

```
{
    "area": "Livingroom",
    "attributes": {
        "brightness": 0,
        "state": "off"
    },
    "entity_id": "4491b6b2-8425-487a-0921-d4d9d69da098",
    "favorite": false,
    "friendly_name": "TV Light",
    "integration": "homey",
    "supported_features": ["BRIGHTNESS", "COLORTEMP", "COLOR"],
    "type": "light"
}
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
