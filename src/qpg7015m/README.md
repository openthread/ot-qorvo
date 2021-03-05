# OpenThread on Qorvo QPG7015M Example

This directory contains example platform drivers for Qorvo QPG7015M on RPi.

## Toolchain

This example uses the GNU GCC toolchain on the Raspberry Pi. To build on the Pi:

1. Download the repo to the Pi
2. Download and install the [GNU toolchain for ARM Cortex-A][gnu-toolchain].

[gnu-toolchain]: https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-a

Note that you may need to install additional packages to make this build work, depending on your actual RPi OS version. The build process will complain if additional packages are required.

## Build Examples

```bash
$ cd <path-to-ot-qorvo>
$ ./script/build qpg7105m
```

After a successful build, the `elf` files are found in `<path-to-ot-qorvo>/build/bin`.

Building a variant which interfaces via a tcp socket is also possible. Replace the uart-posix.c with uart-socket.c in the Makefile.am from examples/platforms/qpg7015m/Makefile.am and rebuild. Now it should be possible to open a telnet to socket 9190 of the raspberry pi from a remote PC. This also easier testing with the official Thread Test Harness.

## Interact

1. Spawn the process:

```bash
$ cd <path-to-ot-qorvo>/build/bin
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
