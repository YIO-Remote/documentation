---
title: "API"
description: "YIO is providing several API Services. To find out more about the different APIs follow the documentation beneth."
menu:
  developers:
    parent: "developers-api"
weight: 10
---

YIO is providing several API Services. To find out more about the different APIs follow the documentation beneth.

## YIO Config API

The configuration is stored in `config.json` on the boot partition of the SD card.

### Accessing the configuration

The configuration class has the following methods:

`config.read` gives a JSON object. You can also access specific keys within this object: `config.read.settings.darkmode` for example.

`config.write` needs a JSON object to write. Example: `config.write = newconfig;` Please note, that this has to be the full configuration. If you want to change part of a config, do the following:

      var tmp = config.read;
      tmp.settings.darmkmode = false;
      config.write = tmp;

`config.readConfig(<path to config file>)` reads the config file from a file.

`config.writeConfig()` saves the config to the config file.

From C++ side, you can access the same methods:

- include the header file:

      #include "config.h

- call a function

      Config::getInstance()->read();

## YIO Dock API

There's a websocket based API for communicating with the dock, running on port 946 `ws://<IP>:946`. The API is used to configure the dock, let it know the remote needs charging and for sending / receiving infrared signals.  
Messages are sent in JSON format.

### Authentication

When a client connects, the remote sends a JSON message:

      {
        "type":"auth_required"
      }

The client needs to respond with:

      {
        "type":"auth",
        "token": "<token>"
      }

If you don't provide a token or the token is invalid, you get an error message:

      {
        "type":"auth",
        "message": "Invalid token"
      }

or

      {
        "type":"auth",
        "message": "Token needed"
      }

Default authentication token is "0"

      {
        "type":"auth",
        "token": "0"
      }

After successful authentication the client is flagged to be able to communicate with the API.

### API Commands

Configure dock friendly name:

      {
        "type":"dock",
        "command":"set_friendly_name",
        "friendly_name":"<Preffered Name>"
      }

Configure dock LED brightness:

      {
        "type":"dock",
        "command":"led_brightness_start",
        "brightness":100
      }

Stop LED animation:

      {
        "type":"dock",
        "command":"led_brightness_stop"
      }

Receive IR Signals:

      {
        "type":"dock",
        "command":"ir_receive_on"
      }

Stop receiving IR Signals:

      {
        "type":"dock",
        "command":"ir_receive_off"
      }

Send IR Signals:

      {
        "type":"dock",
        "command":"ir_send",
        "code":""
      }

      The code can either be comma seperated RAW signals or PRONTO Hex.

Reset dock configuration do default and reboot:

      {
        "type":"dock",
        "command":"reset"
      }

Inform the dock that the remote battery is low and needs charging (for LED indication)

      {
        "type":"dock",
        "command":"remote_lowbattery"
      }

Inform the dock that the remote battery is charged.

      {
        "type":"dock",
        "command":"remote_charged"
      }

## YIO Remote API

There's a websocket based API for communicating with the remote, running on port 946(YIO). Mainly used by the web configurator tool and the YIO Dock.  
The API automatically starts on power up and is always running in the background. The API is stopped, before wifi is turned off and started, after wifi is turned on.  
Messages are sent in JSON format.

### Using the API

You can access the API from the QML side:

      api.start();

or from C++ side:

- include the header file:

      #include "yioapi.h

- call a function:

      YioAPI::getInstance()->start();

### Authentication

When a client connects, the remote sends a JSON message:

      {
        "type":"auth_required"
      }

The client needs to respond with:

      {
        "type":"auth",
        "token": "<token>"
      }

If you don't provide a token or the token is invalid, you get an error message:

      {
        "type":"auth",
        "message": "Invalid token"
      }

or

      {
        "type":"auth",
        "message": "Token needed"
      }

After successful authentication the client is flagged to be able to communicate with the API.

### Messages

When a message arrives from a flagged client, it gets emitted via the `messageReceived` signal. You can connect to this signal from QML or C++ to further process the message.

### Sending messages

You can send messages to all authenticated clients with the `sendMessage(QString msg)` method. The message should be stringified JSON.
