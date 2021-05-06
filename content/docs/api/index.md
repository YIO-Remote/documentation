---
title: "Remote API"
description: "There's a websocket based API for communicating with the remote, running on port 946(YIO). Mainly used by the web configurator tool and the YIO Dock."
menu:
  docs:
    parent: "home"
weight: 10
---

There's a websocket based API for communicating with the remote, running on port 946(YIO). Mainly used by the web configurator tool and the YIO Dock.

The API automatically starts on power up and is always running in the background. The API is stopped, before wifi is turned off and started, after wifi is turned on.  
Messages are sent in JSON format.

## Remote

### Authentication

When a client connects, the remote sends a JSON message:

    {
        "type": "auth_required"
    }

The client needs to respond with:

    {
        "type": "auth",
        "token": <token>
    }

If you don't provide a token or the token is invalid, you get an error message and the connection is terminated:

    {
        "type": "auth",
        "message": "Invalid token"
    }

or

    {
        "type": "auth",
        "message": "Token needed"
    }

After successful authentication the client is flagged to be able to communicate with the API, until the client is disconnected. Client needs to authenticate after reconnecting.

### Commands

Commands are sent to the API as JSON. Not all commands have a response. Commands either return true or false and the payload if there's any.

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result",
        "message": "Optional message for the error."
    }

Some commands return a message as part of the failed execution of the command to explain the error.

## System

### Reboot remote

Command:

    {
        "type": "reboot"
    }

No response. The connection will be disconnected when the remote reboots.

### Shutdown remote

Command:

    {
        "type": "shutdown"
    }

No response. The connection will be disconnected when the remote shuts down.

## Configuration

### Get configuration (the whole configuration)

Command:

    {
        "id": 1,
        "type": "get_config"
    }

Response:

    {
        "id": 1,
        "success": true,
        "type": "result",
        "config": <config>
    }

The whole config as json.

### Set configuration (the whole configuration)

Command:

    {
        "id": 18,
        "type": "set_config",
        "config": <config>
    }

The whole config as json.

Response on success:

    {
        "id": 18,
        "success": true,
        "type": "result",
    }

Response on failure:

    {
        "id": 18,
        "success": false,
        "type": "result",
    }

Setting a new config will reload the UI, but won't add entities or integrations to the memory databases. A reboot is needed after uploading a whole new configuration.

## Integrations

### Discover integrations on the network

Scans for all supported integrations.

### Discover one specific integration on the network

Scans for one specific integration.

### Get supported integrations (list of supported integrations)

Command:

    {
        "id": 22,
        "type": "get_supported_integrations"
    }

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result",
        "supported_integrations": [
            "homeassistant",
            "homey",
            "spotify"
        ]
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Get loaded integrations (json of loaded integrations)

Returns the integrations loaded by the remote from the configuration.

Command:

    {
        "id": 22,
        "type": "get_loaded_integrations"
    }

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result",
        "loaded_integrations": {
            "homeassistant": {
                "data": [
                    {
                        "data": {
                            "ip": "192.168.0.10:8123",
                            "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9"
                        },
                        "friendly_name": "Home Assistant",
                        "id": "homeassistant"
                    }
                ],
                "mdns": "_hap._tcp"
            },
            "spotify": {
                "data": [
                    {
                        "data": {
                            "access_token": "lCzbkuM8wGgCo0FiE",
                            "client_id": "dklfj4897r8912up48fjoi398puo",
                            "client_secret": "fdajh42907t49128ufioprwh",
                            "entity_id": "spotify.spotify",
                            "refresh_token": "zXQktUpnJoyGY"
                        },
                        "friendly_name": "Spotify",
                        "id": "spotify"
                    }
                ],
                "mdns": "_spotify-connect._tcp"
            }
        }
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Get data required to set up an integration

Returns the data required to set up an integration.

Command:

    {
        "id": 22,
        "type": "get_integration_setup_data",
        "integration: "homeassistant"
    }

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result",
        "data": {
            "ip": "",
            "token", ""
        }
    }

Returns a json with all required data fields for the integration. If no data is required, returns and empty object.

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Add integration

Add a new integration to the config file and load it to the memory database

Command:

    {
        "id": 22,
        "type": "add_integration",
        "config": {
            "type": "homeassistant",
            "id": "homeassistant_pro",
            "friendly_name": "My Home Assistant Server",
            "data": {
                "ip": "192.168.0.2",
                "token": "832748hfjkdfu21ytr79218ohfi2"
            }
        }
    }

`type` is the type of integration from supported integrations.
`id` a unique id
`friendly_name` how the integration should show up on the UI
`data` required data fields to configure the integration

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Remove integration

Remove an integration from the config file and the memory database

Command:

    {
        "id": 22,
        "type": "remove_integration",
        "integration_id": "homeassistant_pro"
    }

`integration_id` is the unique id of the integration

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

Adding and removing an integration will be reflected in the UI. (In Settings/Integrations)

## Entities

### Get supported entities (list of supported entities)

Command:

    {
        "id": 22,
        "type": "get_supported_entities"
    }

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result",
        "supported_entities": [
            "light",
            "blind",
            "media_player",
            "climate"
        ]
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Get loaded entities (json of loaded entities)

Returns the entities loaded by the remote from the configuration.

Command:

    {
        "id": 22,
        "type": "get_loaded_entities"
    }

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result",
        "loaded_entities": {
            "blind": [
                {
                    "area": "Living room",
                    "entity_id": "cover.living_room_blinds_level",
                    "friendly_name": "Living room blinds",
                    "integration": "homeassistant",
                    "supported_features": [
                        "OPEN",
                        "CLOSE",
                        "STOP",
                        "POSITION"
                    ]
                }
            ],
            "light": [
                {
                "area": "Entrance",
                    "entity_id": "light.entrance_lamp_level",
                    "friendly_name": "Entrance lamp",
                    "integration": "homeassistant",
                    "supported_features": [
                        "BRIGHTNESS"
                    ]
                },
                {
                    "area": "Kitchen",
                    "entity_id": "light.kitchen_dimmer_level",
                    "friendly_name": "Kitchen lamp",
                    "integration": "homeassistant",
                    "supported_features": [
                        "BRIGHTNESS",
                        "COLOR"
                    ]
                }
            ]
        }
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Add entity

Add a new entity to the config file and load it to the memory database

Command:

    {
        "id": 22,
        "type": "add_entity",
        "config": {
            "type": "light",
            "entity_id": "light.entrance_lamp_level",
            "friendly_name": "Entrance lamp",
            "integration": "homeassistant",
            "supported_features": [
                    "BRIGHTNESS",
                    "COLOR"
            ]
        }
    }

`type` is the type of entity
`entity_id` a unique id
`friendly_name` how the entity should show up on the UI
`integration` the integration that handles the entity
`supported_features` what features the entity has. This information usually comes from the integration. Leave blank if not known.

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Remove entity

Remove an entity from the config file and the memory database

Command:

    {
        "id": 22,
        "type": "remove_entity",
        "entity_id": "light.entrance_lamp_level"
    }

`entity_id` is the unique id of the entity

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

Adding or removing an entity has no effect on the UI. The user needs to add it to the UI via profiles.

## Profiles

### Get all profiles (json of all profiles)

Command:

    {
        "id": 22,
        "type": "get_all_profiles"
    }

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result",
        "profiles": {
            "0": {
                "favorites": [
                    "spotify.spotify",
                    "light.living_room_lamp_level",
                    "light.living_room_corner_lamp_level",
                    "light.kitchen_dimmer_level",
                    "light.kitchen_table_lamp_level",
                    "light.desk_lamp"
                ],
                "name": "Marton",
                "pages": [
                    "favorites",
                    "5",
                    "1",
                    "0",
                    "6",
                    "settings"
                ]
            },
            "1": {
                "favorites": [
                    "media_player.beoplay_m5"
                ],
                "name": "Niels",
                "pages": [
                    "favorites",
                    "5",
                    "6"
                ]
            }
        }
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Set profile

Activates the profile

Command:
{
"id": 22,
"type": "set_profile",
"profile": "0"
}

`profile` is the unique id of the profile

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Add new profile

Create new profile and add it to the config.
Command:

    {
        "id": 22,
        "type": "add_profile",
        "profile": {
            "<uuid>": {
                "favorites": [
                    "media_player.beoplay_m5"
                ],
                "name": "Niels",
                "pages": [
                    "favorites",
                    "5",
                    "6"
                ]
            }
        }
    }

`uuid` is the unique id of the profile
`favorites` if no favorites, leave empty array
`name` name of the profile
`pages` if no pages, leave empty array

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Update profile

Command:

    {
        "id": 22,
        "type": "update_profile",
        "<uuid>": {
                "favorites": [
                    "media_player.beoplay_m5"
                ],
                "name": "Niels",
                "pages": [
                    "favorites",
                    "5",
                    "6"
                ]
        }
    }

`uuid` is the unique id of the profile
`favorites` if no favorites, leave empty array
`name` name of the profile
`pages` if no pages, leave empty array

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Remove profile

Command:

    {
        "id": 22,
        "type": "remove_profile",
        "id": 28491
    }

`id` is the unique id of the profile

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

## Pages

### Get all pages (json of all pages)

Command:

    {
        "id": 22,
        "type": "get_all_pages"
    }

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result",
        "pages": {
            "0": {
                "groups": [
                    "ac9aa887-8d76-440d-b6fa-74ac2cbe8aed",
                    "ac9aa887-8d76-440d-b6fa-74ac2cbe9aed"
                ],
                "image": "/boot/livingroom.jpg",
                "name": "Living room"
            },
            "1": {
                "groups": [
                    "ac9aa887-8d66-440d-b6fa-74ac2cbe9aed"
                ],
                "image": "/boot/kitchen.jpg",
                "name": "Kitchen"
            }
        }
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Add new page

Create new page and add it to the config.
Command:

    {
        "id": 22,
        "type": "add_page",
        "page": {
            "<uuid>": {
                "groups": [
                    "ac9aa887-8d76-440d-b6fa-74ac2cbe8aed",
                    "ac9aa887-8d76-440d-b6fa-74ac2cbe9aed"
                ],
                "image": "/boot/livingroom.jpg",
                "name": "Living room"
            }
        }
    }

`uuid` is the unique id of the page
`groups` the groups the page contains
`name` name of the page
`image` header image of the page

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Update page

Command:

    {
        "id": 22,
        "type": "update_page",
        "<uuid>": {
                "groups": [
                    "ac9aa887-8d76-440d-b6fa-74ac2cbe8aed",
                    "ac9aa887-8d76-440d-b6fa-74ac2cbe9aed"
                ],
                "image": "/boot/livingroom.jpg",
                "name": "Living room"
        }
    }

`uuid` is the unique id of the page
`groups` the groups the page contains
`name` name of the page
`image` header image of the page

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Remove page

Command:

    {
        "id": 22,
        "type": "remove_page",
        "id": "ac9aa887-8d66-440d-b6fa-74ac2cbe9a2d"
    }

`id` is the unique id of the page

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

## Groups

### Get all groups (json of all groups)

Command:

    {
        "id": 22,
        "type": "get_all_groups"
    }

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result",
        "groups": {
            "ac9aa887-8d66-440d-b6fa-74ac2cbe9aed": {
                "entities": [
                    "climate.kitchen_radiator_heating_1"
                ],
                "name": "Heating",
                "switch": false
            },
            "ac9aa887-8d66-570d-b6fa-74ac2cbe9aed": {
                "entities": [
                    "climate.bedroom_radiator_heating_1"
                ],
                "name": "Heating",
                "switch": false
            }
        }
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Add new group

Create new group and add it to the config.
Command:

    {
        "id": 22,
        "type": "add_group",
        "group": {
            "<uuid>": {
                "entities": [
                    "climate.bedroom_radiator_heating_1"
                ],
                "name": "Heating"
            }
        }
    }

`uuid` is the unique id of the group
`entitie` entities that the group contains
`name` name of the group

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Update group

Command:

    {
        "id": 22,
        "type": "update_group",
        "<uuid>": {
                "entities": [
                    "climate.bedroom_radiator_heating_1"
                ],
                "name": "Heating"
        }
    }

`uuid` is the unique id of the group
`entitie` entities that the group contains
`name` name of the group

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Remove group

Command:

    {
        "id": 22,
        "type": "remove_group",
        "id": "ac9aa887-8d66-440d-b6fa-74ac2cbe9aed"
    }

`id` is the unique id of the group

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

## Settings

### Get language list (list of all languages)

Command:

    {
        "id": 22,
        "type": "get_languages"
    }

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
        "languages": [
            {
                "name": "български",
                "id": "bg_BG"
            },
            {
                "name": "Català",
                "id": "ca_ES"
            },
            {
                "name": "Čeština",
                "id": "cs_CZ"
            }
        ]
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Set language

Command:

    {
        "id": 22,
        "type": "set_language",
        "language": "bg_BG"
    }

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Set auto brightness

Command:

    {
        "id": 22,
        "type": "set_auto_brightness",
        "value": true
    }

`value` true or false for on and off

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }

### Set dark mode

Command:

    {
        "id": 22,
        "type": "set_dark_mode",
        "value": true
    }

`value` true or false for on and off

Response on success:

    {
        "id": 22,
        "success": true,
        "type": "result"
    }

Response on failure:

    {
        "id": 22,
        "success": false,
        "type": "result"
    }
