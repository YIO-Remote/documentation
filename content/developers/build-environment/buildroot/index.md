---
title: "Buildroot"
description: "The YIO Remote uses a custom Linux OS for the Raspberry Pi Zero board built with Buildroot"
menu:
  developers:
    parent: "build-environment"
weight: 30
---

The YIO Remote uses a custom Linux OS for the Raspberry Pi Zero board built with [Buildroot](https://www.buildroot.org/). See [remote-os repository](https://github.com/YIO-Remote/remote-os/) for details.

At the moment Buildroot is only working on Linux. Our attempts to get it running on macOS have been unsuccessful and on Windows we don't have any expertise. Please contact us if you have a working solution.

The easiest way to use Buildroot on any system is with the provided Docker YIO build image. See [Docker Build Image for YIO-Remote](https://github.com/YIO-Remote/docker-build) for more information.

This documentation is about installing and using Buildroot on Linux.

## Build Environment Variables

The following optional environment variables control where build artifacts are stored:

| Variable         | Description                                                   |
| ---------------- | ------------------------------------------------------------- |
| `BR2_DL_DIR`     | Buildroot download directory. Default: `$HOME/buildroot/dl`   |
| `BR2_CCACHE_DIR` | Buildroot ccache directory. Default: `$HOME/buildroot/ccache` |

Variables prefixed with `BR2_` are official Buildroot variables. See [environment variables in the Buildroot user manual](https://buildroot.org/downloads/manual/manual.html#env-vars).

## Initial Checkout and Toolchain Build

The [YIO remote-os project](https://github.com/YIO-Remote/remote-os) builds the complete SD card image for the RPi Zero device in the remote. Furthermore, an external toolchain can be built with Buildroot for cross compilation in Qt Creator and the command line. See "External toolchain backend" in the [Buildroot documentation](https://buildroot.org/downloads/manual/manual.html#_cross_compilation_toolchain) for more information.

Notes:

- The most up-to-date version is in the master branch.
- Releases are created from the master branch through a version tag.
- All other branches in remote-os are either upcoming pull requests from the core development team, or the "next big feature" we are working on.  
  Consider them experimental: they might not work and removed or rebased at any time!
- The `Makefile` in the root directory is a build helper for commonly used Buildroot make commands.

Checkout project (if not yet done) and build external cross compiler toolchain:

    # define root directory for project checkout
    SRC_DIR=~/projects/yio

    mkdir -p $SRC_DIR
    cd $SRC_DIR
    git clone https://github.com/YIO-Remote/remote-os.git

    cd $SRC_DIR/remote-os

    # build external toolchain
    make remote-sdk

This will take at least an hour or much longer on a slower system.  
A successful build will end with the following messages:

```
>>>   Rendering the SDK relocatable
PER_PACKAGE_DIR=~/projects/yio/remote-os/buildroot/output/per-package ~/projects/yio/remote-os/buildroot/support/scripts/fix-rpath host
PER_PACKAGE_DIR=~/projects/yio/remote-os/buildroot/output/per-package ~/projects/yio/remote-os/buildroot/support/scripts/fix-rpath staging
/usr/bin/install -m 755 ~/projects/yio/remote-os/buildroot/support/misc/relocate-sdk.sh ~/projects/yio/remote-os/buildroot/output/host/relocate-sdk.sh
mkdir -p ~/projects/yio/remote-os/buildroot/output/host/share/buildroot
echo ~/projects/yio/remote-os/buildroot/output/host > ~/projects/yio/remote-os/buildroot/output/host/share/buildroot/sdk-location
>>>   Generating SDK tarball
~/projects/yio/remote-os/buildroot/output/host/bin/tar czf "~/projects/yio/remote-os/buildroot/output/images/arm-buildroot-linux-gnueabihf_sdk-buildroot.tar.gz" \
        --owner=0 --group=0 --numeric-owner \
        --transform='s#^home/mzehnder/projects/yio/remote-os/buildroot/output/host#arm-buildroot-linux-gnueabihf_sdk-buildroot#' \
        -C / home/mzehnder/projects/yio/remote-os/buildroot/output/host
make[1]: Leaving directory '~/projects/yio/remote-os/buildroot'
```

The SDK will be written to: `./buildroot/output/images/arm-buildroot-linux-gnueabihf_sdk-buildroot.tar.gz`

## Build SD Card Image

    cd $SRC_DIR/remote-os
    make remote

A successful build will end with the following messages:

    Cleaning up partition image files ...
    make[1]: Leaving directory '/home/yio/projects/yio/remote-os/buildroot'
    cp -f /home/yio/projects/yio/remote-os/buildroot/output/images/yio-* /home/yio/projects/yio/remote-os/release/
    # Do not clean when building for one target
    Finished building board: remote

Hint: redirect the `make` output log into a logfile to easy find an error during building or when using `screen` without scrollback capability:

    make 2>&1 | tee remote-os_build_$(date +"%Y%m%d_%H%M%S").log

The final SD card image will be written to:

- Buildroot output: `./buildroot/output/images/yio-remote-sdcard-$VERSION.img`  
  Attention: `make clean` will remove the Buildroot output folder.
- Release folder: `./release/yio-remote-sdcard-$VERSION.img`

The `$VERSION` information consists of: `VersionTag[-CommitCountSinceVersionTag-LastGitHash[-dirty]]`  
Examples:

- `yio-remote-sdcard-v1.1.0.img`:
  - Image relates to version `v1.1.0`, which is `git checkout v1.1.0`
- `yio-remote-sdcard-v1.1.0-19-g07e61834.img`:
  - 19 commits after last release v1.1.0
  - last git commit is g07e61834
- `yio-remote-sdcard-v1.1.0-20-gdbca8b7c-dirty.img`:
  - 20 commits after last release v1.1.0
  - last git commit is gdbca8b7c
  - dirty: uncommitted changes in the file system during build

## Buildroot Commands

The main makefile wraps the most important Buildroot make commands and takes care of configuration handling and output directories:

| Command                  | Description                                                                                                             |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------- |
| `make remote`            | Update configuration from the board's defconfig and start build.                                                        |
| `make clean`             | Deletes all of the generated files, including build files and the generated toolchain!                                  |
| `make remote-menuconfig` | Shows the configuration menu with the board's defconfig. All changes will be written back to the project configuration. |
| `make remote-config`     | Update configuration from the board's defconfig.                                                                        |
| `make help`              | Shows all options.                                                                                                      |

The project configuration in `buildroot-extenal/configs/remote_defconfig` is automatically loaded and saved back with `make remote-menuconfig`. Manual `make savedefconfig BR2_DEFCONFIG=...` and `make defconfig BR2_DEFCONFIG=...` commands are no longer required and automatically taken care of!

Specific Buildroot make commands can still be run in the ./buildroot subdirectory after applying the board configuration.  
For example configuring the Linux kernel with `make linux-menuconfig`.  
Attention: all configuration commands run within the ./buildroot directory will not be persisted to the configuration files stored under ./buildroot-external. This must be done manually!

## Write SD Card Image

Use [balenaEtcher](https://www.balena.io/etcher/) - available for Linux, macOS and Windows - or your favorite tool.
