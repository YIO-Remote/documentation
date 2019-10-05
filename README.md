# YIO (Your Input/Output) touchscreen remote

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

Our documentation repository is split into sections as visible below:
 - [Dock Software](https://github.com/YIO-Remote/documentation#dock-software)
 - [Hardware](https://github.com/YIO-Remote/documentation#hardware)
 - [Integrations](https://github.com/YIO-Remote/documentation#integrations)
   * [Home Assistant]
   * [Homey]
   * [openHAB]
- [Operating System](https://github.com/YIO-Remote/documentation#operating-system)
- [Remote Software](https://github.com/YIO-Remote/documentation#remote-software)
- [web Configuration](https://github.com/YIO-Remote/documentation#web-configuration)

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
  * **3.5" high resolution capacitive touchsreen** (resolution used 480x800px)
  * **ambient light sensor** (detects you light scenario and dims / brighten the display)
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

### Homey
Second SmartHome Backend System that is used to develop the YIO Sofware with.  
The currently supported devices are:
- [x] Blinds
- [ ] Heating
- [x] Lights
- [ ] Media players
- [ ] PVR / TV

### openHAB
Third SmartHome Backend System that is used to develop the YIO Sofware with (currently under initial development).  
The currently supported devices are:
- [ ] Blinds
- [ ] Heating
- [ ] Lights
- [ ] Media players
- [ ] PVR / TV

## Operating System
Due to the used Raspberry Pi Zero W it was choosen to use Buildroot as the Operating System underneath out qt UI.

## Remote Software
In the Remote Software repository you can find the latest build of the YIO Remote Software.  
Later you will find Information how to upload the Software to your YIO Remote here.

## web Configuration
YIO will provide you a self hosted, localy available website where you are able to change the configuration of your Setup.  
Following features are currently planned:
- [ ] **Display config.json** - View your actual used config.json file
- [ ] **Download config.json** - make an easy backup of the complete remote config
- [ ] **Modify config.json** - Easy edit of the file from every browser in local Network
- [ ] **Setup Wizard** - Answere short Questions and build the config.json file with the help of our wizard
- [ ] **Upload config.json** - restore you remote to this config