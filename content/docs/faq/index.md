---
title: "Frequently Asked Questions"
description: "Frequently asked questions about the YIO Remote."
menu:
  docs:
    parent: "home"
weight: 30
---

## Can I update the Dock Firmware manually?

Yes, you need to compile or download a new dock firmware image. Then use `curl` to push the firmware to the dock with the following command:

    curl -F "file=@firmware.bin" http://<Dock IP>/update

You can also use [ESPHome-Flasher](https://github.com/esphome/esphome-flasher) to update the firmware via USB.

## How can I change the timezone or set the time manually?

### remote-os v2

The default system timezone is set to UTC. The timezone for the app can be set in the file `/boot/timezone`. This file is on the FAT boot partition and accessible from the mounted SD card.

The content of `/boot/timezone` is a single line containing the Linux TZ name.  
 See <https://en.wikipedia.org/wiki/List_of_tz_database_time_zones> for a list of valid TZ database names. They can also be retrieved with `timedatectl list-timezones`. Examples:

- Europe/Copenhagen
- Asia/Hong_Kong
- America/Vancouver

### remote-os < 2.0.0

At the moment you can only set the timezone or current time in a ssh shell with the following commands:

- Time zone:
  - List timezones: `timedatectl list-timezones`
  - Set timezone example: `timedatectl set-timezone America/Los_Angeles`
- Date and time:
  - Show current time: `timedatectl`
  - Set date and time: `timedatectl set-time YYYY-MM-DD HH:MM:SS`  
     Example: `timedatectl set-time '2020-09-08 03:19:00'`
