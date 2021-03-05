# OpenThread on Qorvo GP712 Example

This directory contains example platform drivers for Qorvo GP712 on RPi.

## Toolchain

This example uses the GNU GCC toolchain on the Raspberry Pi. To build on the Pi:

1. Download the repo to the Pi
2. Download and install the [GNU toolchain for ARM Cortex-A][gnu-toolchain].

[gnu-toolchain]: https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-a

Note that you may need to install additional packages to make this build work, depending on your actual RPi OS version. The build process will complain if additional packages are required.

## Build Examples

```bash
$ cd <path-to-ot-qorvo>
$ ./script/build gp712
```

After a successful build, the `elf` files are found in `<path-to-openthread>/build/bin`.

## Interact

1. Spawn the process:

```bash
$ cd <path-to-openthread>/bulid/bin
$ ./ot-cli-ftd
```

2. Type `help` for list of commands.

```bash
> help
help
channel
childtimeout
contextreusedelay
extaddr
extpanid
ipaddr
keysequence
leaderweight
masterkey
mode
netdata register
networkidtimeout
networkname
panid
ping
prefix
releaserouterid
rloc16
route
routerupgradethreshold
scan
start
state
stop
```
