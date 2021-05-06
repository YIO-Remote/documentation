---
title: "Dock"
description: "The YIO Dock charges the remote and functions as an IR receiver and blaster."
menu:
  docs:
    parent: "hardware"
weight: 10
---

The YIO Dock charges the remote and functions as an IR receiver and blaster.

## YIO Dock Hardware

- **USB-C connector** (works with all chargers that adhering the USB-C Standard)
- **IR-Receiver** (located at the top, let you learn IR codes)
- **4 IR LEDs** (can emit RAW & Pronto HEX IR codes)
- **headphone jack** (IR Extender can be connected in parallel to this Port)

## Powering the YIO dock

The YIO dock has an USB-C connector and works with power supplies adhering to the USB-C standard. Make sure you use a power supply that can do 3A @ 5V.

## IR capabilities

There are 4 IR LEDs and one IR receiver on the top of the dock. There is a headphone jack for connecting IR extenders. This jack is connected in parallel with the 4 IR LEDs and also has a 51 ohm resistor.

RAW and Pronto code formats are supported for IR sending.

## Explanation of dock light

#### During setup

| State                 | Light                     |
| --------------------- | ------------------------- |
| Needs setup           | Blinking (1s)             |
| Connecting to WiFi    | Blinking (0.2s)           |
| Connection successful | Three quick blinks (0.1s) |

### Normal usage

| State                 | Light          |
| --------------------- | -------------- |
| Battery low           | Blink every 4s |
| Battery charging      | Pulsing        |
| Battery fully charged | Constant light |
| Otherwise             | No light       |

## Update firmware

Download the firmware from:
[github.com/YIO-Remote/dock-software/releases](https://github.com/YIO-Remote/dock-software/releases)

You can use [ESPHome-Flasher](https://github.com/esphome/esphome-flasher) to update the firmware via USB or use `curl` to perform an OTA update. Navigate to the extracted zip file and run the following command:

`curl -F "file=@firmware.bin" http://yio-dock-something.local/update`

Of course **replace** the address with the IP address or hostname of your YIO
dock. You will see the dock LED blink during the update process.

❗️ The OTA update method might not work with the Kickstarter units. Please use the USB flash method instead.

It might happen that the dock forgets the WiFi credentials. In this case you
need to send them over via Bluetooth. The WiFi access point was removed due to
stability issues.

Connect to the dock via Bluetooth and send the following `json` via the
Bluetooth serial interface:

```json
{
  "ssid": "your_ssid",
  "password": "your_wifi_password"
}
```

After this the dock should connect to your network again.

### Send WiFi credentials over Bluetooth (Mac)

You can connect to the dock from your Mac via Bluetooth. Go to System
preferences/Bluetooth and wait for the dock to show up. For best results make
sure the dock is close to your Mac, _the closer the better_. It might take a
couple of minutes to show up. If it won’t show up, you can try the following:

1. Click in the area where devices should show up
2. Turn Bluetooth off/on on your Mac
3. Go back to System preferences main menu and then enter Bluetooth settings
   again

When it’s there, click on connect. After this a device should show up in
`/dev/`. After that you can just echo to that device:

`echo "{\"ssid\":\"your_ssid\", \"password\":\"your_password\"}" > /dev/cu.YIO-something`

Don’t forget to escape the `"` as it’s shown above.
