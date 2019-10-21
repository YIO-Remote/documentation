# YIO (Your Input/Output) Touchscreen remote

## About YIO
YIO is made by a smarthome enthusiast, it allows the user to directly interact with a smarthome hub system and extend its usage to the feel and response of a remote.  
You can find more Information about (or contribute to) YIO in this documentation repository or via the following links:

- [YIO Homepage](https://yio-remote.com)  
- [YIO Discord Channel](http://chat.yio-remote.com)  
- [YIO Facebook](https://www.facebook.com/YIOremote)  
- [YIO Instagram](https://www.instagram.com/yioremote/)  
- [YIO Twitter](https://twitter.com/yioremote)  
- [YIO YouTube](http://video.yio-remote.com/)  
- [YIO Translation](https://translate.yio-remote.com) [![Crowdin](https://d322cqt584bo4o.cloudfront.net/yio-remote-translation/localized.svg)](https://crowdin.com/project/yio-remote-translation)

Our **documentation repository** is split into sections as visible below:
 - [Dock Software](https://github.com/YIO-Remote/documentation#dock-software)
 - [Hardware](https://github.com/YIO-Remote/documentation#hardware)
 - [Integrations](https://github.com/YIO-Remote/documentation#integrations)
   * [Home Assistant](https://github.com/YIO-Remote/documentation#home-assistant)
   * [Homey](https://github.com/YIO-Remote/documentation#homey)
   * [openHAB](https://github.com/YIO-Remote/documentation#openhab)
- [Operating System](https://github.com/YIO-Remote/documentation#operating-system)
- [Remote Software](https://github.com/YIO-Remote/documentation#remote-software)
- [Web Configuration](https://github.com/YIO-Remote/documentation#web-configuration)

Additional helpful documentation can be found for
- [YIO API](https://github.com/YIO-Remote/documentation/blob/master/yio-api.md)
- [Homey Integration](https://github.com/YIO-Remote/documentation/blob/master/integration.homey.md)
- [IR Integration](https://github.com/YIO-Remote/documentation/blob/master/integration.ir.md)

## Dock Software
In the Dock Software repository you can find the latest build of the YIO Dock Software.  
Later you will find Information how to upload the Software to your YIO Dock here.

## Hardware
The Hardware currently consists of the following two main parts:

- **The YIO Dock** charges the remote quickly and functions as an IR receiver and blaster.
  * **USB-C connector** (works with all chargers that adhering the USB-C Standard)
  * **IR-Receiver** (located at the top, let you learn IR codes)
  * **4 IR LEDs** (can emit RAW & Pronto HEX IR codes)
  * **headphone jack** (IR Extender can be connected in parallel to this Port)

- **YIO Remote** the [HID](https://en.wikipedia.org/wiki/Human_interface_device) to interact with your SmartHome backend
  * based on a **Raspberry Pi Zero W** (used as mainboard and prived connections via Wi-Fi and Bluetooth)
  * **13 customizable physical buttons** (2 different functions at each button can be assigned)
  * **3.5" high resolution capacitive touchsreen** (480x800px resolution)
  * **ambient light sensor** (detects you light scenario and dims / brightens the display)
  * **battery monitoring** (charge, health & voltage)
  * **custom designed housing** (CNC milled, sandblasted and anodized)
  * **gesture sensor** (detects you and switches on the display)
  * **haptic feedback motor** (gives vibrating feedback)
  * **proximity sensor** (detects proximity and switches on the display)
  

## Integrations
In general the YIO Remote can be used with all SmartHome backend systems, currently maintained Integrations are listed here, if you would like to be added with your Integration, feel free to contact us.

### Home Assistant
Main SmartHome Backend System that is used to develop the YIO Sofware with.  
The currently supported devices are:
- [x] Blinds
- [ ] Heating
- [x] Lights
- [x] Media players
- [ ] PVR / TV

Example config.json file for Home Assistant Integration will be available later.

### Homey
Second SmartHome Backend System that is used to develop the YIO Sofware with.  
The currently supported devices are:
- [x] Blinds
- [ ] Heating
- [x] Lights
- [x] Media players
- [ ] PVR / TV

Example config.json file for Homey Integration will be available later.

### Infrared (IR)
IR Integration supporting IR code blasting via the YIO Dock.  
The currently supported devices are:
- [x] PVR / TV

Example config.json file for IR Integration will be available later.

### openHAB
Third SmartHome Backend System that is used to develop the YIO Sofware with (currently under initial development).  
The currently supported devices are:
- [ ] Blinds
- [ ] Heating
- [ ] Lights
- [ ] Media players
- [ ] PVR / TV

Example config.json file for openHAB Integration will be available later.

## Operating System
We choose to use Buildroot to create YIO's underlaying Operating System.  
To build the UI / UX Interface for YIO, the Open Source version of QT is used.

## Remote Software
In the Remote Software repository you can find the latest build of the YIO Remote Software.  
Later you will find Information how to upload the Software to your YIO Remote here.

## Web Configuration
YIO will provide you a self hosted, locally available website where you are able to change the configuration of your Setup.  
Following features are currently planned:
- [x] **Display config.json** - View your actual used config.json file
- [x] **Download config.json** - make an easy backup of the complete remote config
- [x] **Modify config.json** - Easy edit of the file from every browser in local Network
- [x] **Profile management** - Add Profiles, and assign Pages consisting of groups of devices
- [x] **Upload config.json** - restore you remote to this config
- [ ] **Infrared remote tool** - Building IR remotes through the configurator
- [ ] **Adding new integrations** - Ading your favorite integrations and their required settings.
- [ ] **Adding custom entities** - Every device is a entity in the YIO configuration, They describe the device features and link to a integrations.
