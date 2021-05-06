---
title: "Setup Ubuntu Developer VM"
description: "This guide describes the setup of an Ubuntu VM for the complete YIO Remote development"
menu:
  developers:
    parent: "build-environment"
weight: 20
---

This guide describes the setup of an Ubuntu VM for the complete YIO Remote development.  
The development VM supports:

- Building, running and debugging the YIO Remote software on the Linux desktop.
- Creating the custom Linux image for Raspberry Pi Zero using Buildroot.
- Cross compiling the YIO Remote software for Raspberry Pi Zero.
- Using Qt Creator to cross compile and deploy directly to the device.

Since certain aspects are the same for local development on macOS and Windows, this guide focuses on Linux and cross compilation. Common tasks are documented in dedicated Wiki topics and will be referenced in this documentation.

## Prerequisites

Host system:

- Dual core processor or better.
- 8 GB RAM or more recommended.
- 50 GB of free hard drive space.  
  SSD is highly recommended.
- Internet access.

Software:

A virtualization software is required if the Linux system is not installed on a dedicated PC or as dual boot option.  
Recommendations:

- [Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)  
  A cross-platform virtualization application and free for personal use.
- VMware Fusion / Player / Workstation  
  A commercial alternative with usually better desktop integration.
- Windows Hyper-V  
  Included in Windows 10 Pro, but can only be used if no other virtualization software is in use!

Other virtualization software will work too of course. We just list what we are using personally.

### Download Ubuntu

The minimal recommended Ubuntu version is 18.04 LTS.  
Versions 19.04 and 19.10 have been tested as well. If you want long term support without the latest features and software versions, then choose the LTS version.

#### Windows Hyper-V

At the time of writing this guide the only fully working Ubuntu version with Windows Hyper-V was the provided 18.04.3 LTS version within Hyper-V. The newer 19.04 was not working with the advanced Windows integration features like custom screen resolution with RDP!

In Hyper-V Manager choose "Quick Create" and then select "Ubuntu 18.04.3 LTS":

{{< img src="images/Hyper-V_quick-create.png" alt="Hyper-V Quick Create Ubuntu 18.04.3 LTS" caption="<em>Hyper-V Quick Create Ubuntu 18.04.3 LTS</em>" class="border-0" >}}

#### Other Virtualization Software

Download the desired Ubuntu Desktop ISO image from <https://ubuntu.com/download/desktop>.

This guide will use the 19.10 version with VMware Fusion.

## Create new Virtual Machine

Create a new virtual machine from the downloaded ISO image.

1. In VMware select "Install from disc or image" and the creation wizard starts:

   {{< img src="images/Fusion_create1_new-small.png" alt="Create a new Virtual Machine" caption="<em>Create a new Virtual Machine</em>" class="border-0" >}}

1. When using VMWare you can choose "Linux Easy Install" to automatically create a user account during installation.  
   However, for this guide we are not using it. The reason is to show the regular setup process when installing Ubuntu with other virtualization products.

   {{< img src="images/Fusion_create2_easy_install-small.png" alt="Linux Easy Install" caption="<em>Linux Easy Install</em>" class="border-0" >}}

1. In the firmware type page use the default "Legacy BIOS" option:

   {{< img src="images/Fusion_create3_firmware-small.png" alt="Choose Firmware Type" caption="<em>Choose Firmware Type</em>" class="border-0" >}}

1. In the final wizard step click "Customize" and give a better name to the virtual machine like "YIO Development":

   {{< img src="images/Fusion_create4_finish-small.png" alt="Customize VM Name" caption="<em>Customize VM Name</em>" class="border-0" >}}

The creation wizard is now complete.  
Before starting the new VM some settings should be customized though:

- Processor cores: at least 2
- Memory: at least 4 GB

{{< img src="images/Fusion_create5_cpu-mem-small.png" alt="Create a new Virtual Machine" caption="<em>Create a new Virtual Machine</em>" class="border-0" >}}

- Disk space: at least 50 GB

{{< img src="images/Fusion_create6_hd-small.png" alt="Create a new Virtual Machine" caption="<em>Create a new Virtual Machine</em>" class="border-0" >}}

The virtual machine can now be started for the Ubuntu OS installation.

## Install Ubuntu

Start the virtual machine to begin the installation process.

1. Choose your preferred language and click 'Install Ubuntu':

   {{< img src="images/Ubuntu_install1-small.jpg" alt="Create a new Virtual Machine" class="border-0" >}}

2. Choose your keyboard layout and click 'Continue':

   {{< img src="images/Ubuntu_install2-keyboard-small.jpg" alt="Create a new Virtual Machine" class="border-0" >}}

3. Since we like the development as small as possible select minimal installation:

   {{< img src="images/Ubuntu_install3-updates-small.jpg" alt="Create a new Virtual Machine" class="border-0" >}}

4. Use the default for installation type (hard drive partitioning):

   {{< img src="images/Ubuntu_install4-install_type-small.jpg" alt="Create a new Virtual Machine" class="border-0" >}}

5. Select timezone.  
   With a network connection this is usually pre-selected to the correct location:

   {{< img src="images/Ubuntu_install5-timezone-small.jpg" alt="Create a new Virtual Machine" class="border-0" >}}

6. Set the user account information.  
   If you wish you can choose "Log in automatically" if you use the VM solely for YIO development.

   {{< img src="images/Ubuntu_install6-user-small.jpg" alt="Create a new Virtual Machine" class="border-0" >}}

7. Finish the installation with 'Restart Now' and press Enter when asked to eject the installation medium. Otherwise the system won't restart!

   {{< img src="images/Ubuntu_install7-complete-small.jpg" alt="Create a new Virtual Machine" class="border-0" >}}

8. After system reboot you are greeted by the login screen.  
   Select your account and enter your credentials defined in the setup process.

   {{< img src="images/Ubuntu_install8-welcome-small.jpg" alt="Create a new Virtual Machine" class="border-0" >}}

9. At first login a setup wizard is shown.  
   Select desired options or skip through the wizard:

   {{< img src="images/Ubuntu_install9-firstrun-small.jpg" alt="Create a new Virtual Machine" class="border-0" >}}

10. Software updates might be available. Install the updates and restart system if asked to.

The installation of the base Linux system is now finished! Next step is to install the required development tools.

## Install Required Software

1.  Common tools  
    For remote access `openssh-server` and `screen` are installed. This is optional.  
    The `nano` editor is a personal preference. Feel free to install your personal choice.

         sudo apt install \
             bzip2 \
             git \
             nano \
             openssh-server \
             rsync \
             screen \
             ssh \
             tar \
             unzip

2.  Build dependencies for YIO Remote software

        sudo apt-get install \
            libavahi-client-dev \
            libgl1-mesa-dev

3.  Build tools

        sudo apt install \
            build-essential \
            dos2unix \
            g++ \
            gdb-multiarch \
            gettext \
            libncurses5-dev \
            libtool \
            npm \
            patch \
            python \
            texinfo

4.  Optional: Docker

TODO

## Checkout YIO Remote Project Repositories

See [QT Creator IDE Setup: Project Organization and Checkout](developer-qt_ide#project-organization-and-checkout).

## Cross Compile Toolchain for RPi Zero

This step is optional and only required if you want to cross-compile the YIO Remote software for the Raspberry Pi Zero. The crosscompile toolchain is also being used by Qt Creator to build and deploy the YIO Remote software to the RPi Zero device.  
Without the cross compile toolchain you can still build and run the software on the Ubuntu Linux desktop but not on a Raspberry Pi.

The toolchain can either be downloaded or compiled from the [remote-os project](https://github.com/YIO-Remote/remote-os).

### Download pre-compiled Toolchain

See [remote-os releases](https://github.com/YIO-Remote/remote-os/releases) for pre-compiled toolchains.  
Not every remote-os release requires a new toolchain. Usually, major releases will require a new toolchain (v1.y.z, v2, y.z, etc.), or if the Qt version has been updated or new libraries have been introduced.

### Build Toolchain with Buildroot

Start toolchain build:

    cd $YIO_SRC/remote-os
    make remote-sdk

The build will take at least an hour or much longer on a slower system!  
The SDK will be written to: `./buildroot/output/images/arm-buildroot-linux-gnueabihf_sdk-buildroot.tar.gz`

See [Buildroot developer topic](developer-buildroot) for more information.

### Install Cross Compile Toolchain

After download or compiling the cross compile toolchain, it needs to be unpacked and relocated to be used with Qt Creator.

Copy the archive to the desired destination folder (for example ~/projects/yio) and then rename and relocate it:

    tar -xf arm-buildroot-linux-gnueabihf_sdk-buildroot.tar.gz
    mv arm-buildroot-linux-gnueabihf_sdk-buildroot yio-remote-toolchain-vX.Y.Z
    cd yio-remote-toolchain-vX.Y.Z
    ./relocate-sdk.sh

Attention: do not move or rename the toolchain after calling `relocate-sdk.sh`. If you want to move it to another directory, then extract the original archive and execute relocate-sdk.sh again.

## Install Qt Runtime and Qt Development Tools

See [QT Creator IDE Setup](developer-qt_ide.md).

## Qt Creator Configuration for Raspberry Pi

After building the cross-compilation toolchain with Buildroot it can be used in Qt Creator to define a new device target. In our case the RPi Zero.

The required configuration is done through the Options dialog in Qt Creator: Tools, Options... from the main menu.

{{< img src="images/qt-options.png" alt="Qt Options Dialog" caption="<em>Qt Options Dialog</em>" class="border-0" >}}

After installation there's a default Qt kit defined for the current desktop. A kit is a platform definition for a specific device including Qt version, compiler and debugger.  
First we need to add the specific Qt build, cross-compiler and debugger information before defining a new kit for RPi Zero.

### Add Raspberry Pi Qt build

1. Select _Kits_ in the Options dialog list.
2. Select _Qt Versions_ tab.
3. Click _Add..._
4. Select the qmake executable in the RPi Buildroot build:  
   `/home/yio/projects/yio/yio-remote-toolchain-v1.0.0/bin/qmake`
5. Click _Apply_.

{{< img src="images/kit-rpi-qt-version.png" alt="RPi Qt build" caption="<em>RPi Qt build</em>" class="border-0" >}}

### Configure Cross-Compilers

Here we have to reference the relocated Buildroot SDK. Replace `/home/yio/projects/yio/yio-remote-toolchain-v1.0.0` with the location where you have extracted the external Buildroot toolchain.

Note: the screen shots still show the old Buildroot output pathes instead of the relocated SDK!

1. Select _Kits_ in the Options dialog list.
2. Select _Compilers_ tab.

   {{< img src="images/kit-compilers.png" alt="Kits: Compilers" caption="<em>Kits: Compilers</em>" class="border-0" >}}

3. Define C compiler: click _Add_ and select GCC -> C.

   1. Name: `RPi GCC`
   2. Compiler path:  
      `/home/yio/projects/yio/yio-remote-toolchain-v1.0.0/bin/arm-buildroot-linux-gnueabihf-gcc`
   3. The ABI will be set automatically: arm-linux-generic-elf-32bit.
   4. Click _Apply_.

   {{< img src="images/kit-rpi-c-compiler.png" alt="RPi C Compiler" caption="<em>RPi C Compiler</em>" class="border-0" >}}

4. Define C++ compiler: click _Add_ and select GCC -> C++

   1. Name: `RPi g++`
   2. Compiler path:  
      `/home/yio/projects/yio/yio-remote-toolchain-v1.0.0/bin/arm-buildroot-linux-gnueabihf-g++`
   3. The ABI will be set automatically: arm-linux-generic-elf-32bit.
   4. Click _Apply_.

   {{< img src="images/kit-rpi-cpp-compiler.png" alt="RPi C++ Compiler" caption="<em>RPi C++ Compiler</em>" class="border-0" >}}

### Configure Debugger

1. Select _Kits_ in the Options dialog list.
2. Select _Debuggers_ tab.
3. Click _Add_ and enter the following information:

   1. Name: `RPi debugger`
   2. Path: `/usr/bin/gdb-multiarch`
   3. Click _Apply_.

   {{< img src="images/kit-rpi-debugger.png" alt="RPi Debugger" caption="<em>RPi Debugger</em>" class="border-0" >}}

### Define RPi Zero as new Device

1. Select _Devices_ in the Options dialog list.

   {{< img src="images/qt-devices.png" alt="Qt Devices" caption="<em>Qt Devices</em>" class="border-0" >}}

2. Click _Add..._
3. In the dialog box select _Generic Linux Device_ and click _Start Wizard_.

   {{< img src="images/devices-add.png" alt="Generic Linux Device" caption="<em>Generic Linux Device</em>" class="border-0" >}}

4. In the connection page enter the following information:

   - Name: `YIO Remote`
   - Host name or IP address
   - Username: `root`

5. In the Key Deployment page create a new key pair and deploy the public key to the device.

   {{< img src="images/device-create-keypair.png" alt="Create Key Pair" caption="<em>Create Key Pair</em>" class="border-0" >}}

   {{< img src="images/device-key-deployment-success.png" alt="Key Deployment succesful" caption="<em>Key Deployment succesful</em>" class="border-0" >}}

6. Finish the wizard.
7. A device test will be run.  
   For a successful test the device needs at least a sftp service or rsync.

   {{< img src="images/device-test.png" alt="Device Test" caption="<em>Device Test</em>" class="border-0" >}}

8. Close the device test dialog and click _Apply_ in the devices tab.

   {{< img src="images/device-yio-remote.png" alt="YIO Remote device" caption="<em>YIO Remote device</em>" class="border-0" >}}

### Create a Qt Kit with Cross-Compiler Toolchain

Now all the individual pieces are defined and a RPi Qt kit can be configured:

1. Select _Kits_ in the Options dialog list.
2. Select _Kits_ tab.
3. Click the _Add_ button.
4. Enter the following information for the new RPi kit:
   - Name: `RPi Crosscompile`
   - Device Type: `Generic Linux Device`
   - Device: select the previously defined `YIO Remote` device.
   - Sysroot: select the output target of the Buildroot build.  
      `/home/yio/projects/yio/remote-os/rpi0/output/target`
   - Compiler: select the previously defined cross compilers.
     - C: `RPi GCC`
     - C++: `RPi g++`
   - Environment: add environment variable:  
     `QT_LINGUIST_DIR=/home/yio/Qt/5.12.6/gcc_64/bin`  
     Reason: The cross-compiler toolchain doesn't contain the Qt linguist tools. But we can simply use the tools from the desktop kit. See topic [Qt IDE: qmake Environment Variables](developer-qt_ide.md#qmake-environment-variables).
   - Debugger: select the previously defined debugger: `RPi debugger`
   - Qt version: select the previously defined version: `Qt 5.12.4 (host)`
   - Qt mkspec is not required, leave empty.
   - CMake Tool is not required, leave empty.
5. Click Ok to save the new RPi kit.

{{< img src="images/kit-rpi-crosscompile-env.png" alt="RPi Crosscompile Kit" caption="<em>RPi Crosscompile Kit</em>" class="border-0" >}}

### Device Deployment

The required files for remote deployment are defined in the remote.pro project file and cannot be changed in the Qt Creator project settings dialog. They are only shown in Run Settings, Deployment:

{{< img src="images/qt-device-deployment.png" alt="Qt Device Deployment" caption="<em>Qt Device Deployment</em>" class="border-0" >}}

For the remote console to be redirected to Qt Creator the run environment variable `QT_ASSUME_STDERR_HAS_CONSOLE` must be set to 1. Furthermore, the screen parameters and touch driver settings must be set:

| Variable                            | Value                               |
| ----------------------------------- | ----------------------------------- |
| QT_ASSUME_STDERR_HAS_CONSOLE        | 1                                   |
| QT_QPA_EGLFS_FORCE888               | 1                                   |
| QT_QPA_EGLFS_PHYSICAL_WIDTH         | 46                                  |
| QT_QPA_EGLFS_PHYSICAL_HEIGHT        | 76                                  |
| QT_QPA_EVDEV_TOUCHSCREEN_PARAMETERS | /dev/input/event0:rotate=90:invertx |

Select Projects, Click on Run in the 'RPi Crosscompile' kit and define above variables in the Run Environment:

![](images/qt/)
{{< img src="images/rpi-run-environment.png" alt="Run Environment" caption="<em>Run Environment</em>" class="border-0" >}}

To deploy and run on the RPi device select the RPi cross-compile run target:

{{< img src="images/qt-deploy-remote-linux.png" alt="Deploy to Remote Linux Host" caption="<em>Deploy to Remote Linux Host</em>" class="border-0" >}}

### Remote Debugging

The gdb server must be available on the remote device for remote debugging.

_TODO: include gdb server in remote-os build_
