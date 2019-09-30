# How to Flash a Proxmark3 with a Raspberry PI 3

This repository describes how to flash a Proxmark3 with a Raspberry PI 3 by using the JTAG interface of the Pi.

## Install Software

```
apt update
apt upgrade
apt-get install git autoconf libtool make pkg-config libusb-1.0-0 libusb-1.0-0-dev libftdi-dev telnet
```

## Build OpenOCD

```
git clone --recursive git://git.code.sf.net/p/openocd/code openocd-git && cd openocd-git
./bootstrap
./configure --enable-bcm2835gpio --enable-buspirate --enable-maintainer-mode --disable-werror --enable-openjtag_ftdi --prefix=/usr
make
make install
```
## OpenOCD Configuration

```
vi /usr/share/openocd/scripts/interface/raspberrypi123-native.cfg
```
Insert the content of the ```raspberrypi123-native.cfg``` file.

```
vi /usr/share/openocd/proxmark.cfg
```
Insert the content of the ```proxmark.cfg``` file.

## Connect your Proxmark3 with the Raspberry PI 3

The following diagram shows which pins have to be connected to each other.
```
      Raspberry PI 3                    Proxmark3
PIN     GPIO        Name          ->      PIN
19      GPIO10      SPI_MISI      ->      TDI
21      GPIO09      SPI_MISO      ->      TDO
22      GPIO25      CS            ->      TMS
23      GPIO11      SPI_CLK       ->      CLK
39      GND         GND           ->      GND
```
For the power supply of the PM3 it is recommended to use an external power source not the Raspberry PI.

## Flash Proxmark Firmware

Start OpenOCD:

```
openocd -f /usr/share/openocd/proxmark.cfg
```

Connect to OpenOCD with Telnet:

```
telnet 127.0.0.1 4444
```

Flashing the firmware:

```
halt
flash erase_sector 0 0 15
flash erase_sector 1 0 15

flash write_image /<PATH_TO_THE_PROXMARK3_FILES>/fullimage.elf
flash write_image /<PATH_TO_THE_PROXMARK3_FILES>/bootrom.elf
```


## Additional Information and Sources

* For more detailed knowledge about using Raspberry and OpenOCD, I can recommend the following link. [Raspberry Pi and OpenOCD](https://iosoft.blog/2019/01/28/raspberry-pi-openocd/)
* And also, the post about [Repair Pogo E02 with Raspberry PI (1,2 or 3) JTAG and OpenOCD](https://forum.doozan.com/read.php?3,21789,21927).
* [Debricking Proxmark3 with a BusPirate](https://github.com/Proxmark/proxmark3/wiki/Debricking-Proxmark3-with-buspirate)