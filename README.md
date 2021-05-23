# My Microcontroller-setup

## Overview

Setup for developing for microcontrollers of various flavours unders different operating systems.

For one reason or another I've ended up switching between machines and also needing to reinstall 
software after upgrades so I thought it might be a good idea to document the setup used for various
gadgets that I try and develop against.  The devices concerned are

* Arduino
* PIC Microcontrollers
* Raspberry Pi Pico
* Teensy
* TinyFPGA

I'm going to gradually add details and any config to this project as I, yet again, setup tools 
for developing with these devices.

Primary development machines are

* Raspberry Pi 4 8Gb
* MacBook Pro
* Laptop running Ubuntu 20.04



## Raspberry Pi


### General

For schematic capture install with the following command

```
apt install xschem fritzing
```


### Arduino 

```
apt install arduino
```




### Raspberry Pi Pico

Following the instructions in the [Getting Started with C/C++ development](https://datasheets.raspberrypi.org/pico/getting-started-with-pico.pdf) guide

Running the following commands

```
sudo apt update
sudo apt install cmake gcc-arm-none-eabi libnewlib-arm-none-eabi build-essential thonny
sudo apt install xschema fritzing
```

Since I'm connected to my Raspberry Pi over SSH and X11 I don't have a desktop running so the drive that the 
Pico presents on connecting with BOOTSEL pressed doesn't automatically mount.

In order to get it to work I had to mount the drive manually using this

```
sudo mount /dev/sda1 /mnt
```

I was that able to download the [MicroPython install](https://micropython.org/download/rp2-pico/rp2-pico-latest.uf2) and 
copy to the filesystem.

Loosely following the [Getting Started instructions](https://projects.raspberrypi.org/en/projects/getting-started-with-the-pico/3) I
was able to select "MicroPython (Raspberry Pi Pico)" as the interpreter for [Thonny](https://thonny.org) and access the REPL in the 
Pico and run the simple code to switch the LED on/off

```
>>> from machine import Pin
>>> led = Pin(25, Pin.OUT)
>>> led.value(1)
>>> led.value(0)
>>> led.value(1)
```

Wasn't able to get the MicroPython SSD1306 library installed with Thonny though
trying with rshell




## MacOS

### General



### Arduino

Download the Arduino IDE from [here](https://www.arduino.cc/en/software)


### Raspberry Pi Pico

Setting up rshell using a virtual environment

```
python3 -mvenv .matrix
source .matrix/bin/activate
pip install rshell
```

Assuming you followed the [getting started with MicroPython](https://www.raspberrypi.org/documentation/rp2040/getting-started/#getting-started-with-micropython) steps you should be able to plug the Pico via USB into the Mac and it will be accessible from a serial port using the following command

```
rshell -p /dev/tty.usbmodem0000000000001 --buffer-size 512
```

This will give output like this

```
Connecting to /dev/tty.usbmodem0000000000001 (buffer-size 512)...
Trying to connect to REPL  connected
Retrieving sysname ... rp2
Testing if ubinascii.unhexlify exists ... Y
Retrieving root directories ... /ssd1306.py/
Setting time ... May 23, 2021 14:04:53
Evaluating board_name ... pyboard
Retrieving time epoch ... Jan 01, 1970
Welcome to rshell. Use Control-D (or the exit command) to exit rshell.
/Users/sheila/wd/my-microcontroller-setup>
```

This is now the rshell prompt, you can start the REPL by typing `repl` you can now interact with the Python interpreter.

Pressing `CTRL-X` will drop back to the `rshell` shell.



## Notes

### VLAN Setup on Raspberry Pi

Since the Raspberry Pi I have needs to be accessible from two separate subnets (one on a VLAN) I 
needed to add some config.

First install the `vlan` package

```
apt install vlan
```

Created the `/etc/network/interfaces.d/vlan7` file with this contents

```
iface vlan27 inet static
  vlan-raw-device eth0
  address 192.168.227.21
  netmask 255.255.255.0
  
auto vlan27
```

The main `eth0` interface is on `192.168.123.21`

Changed SSH config to only accept keys and added the key from the machine I want to connect from



## Reference

### Arduino
 
* [Nano pinout](https://www.makerguides.com/arduino-nano/)
* [Download IDE](https://www.arduino.cc/en/software)

### Raspberry Pico

* [Getting started](https://www.raspberrypi.org/documentation/rp2040/getting-started/)
* [Getting started with MicroPython](https://www.raspberrypi.org/documentation/rp2040/getting-started/#getting-started-with-micropython)
* [Getting started with C/C++ development](https://datasheets.raspberrypi.org/pico/getting-started-with-pico.pdf)
* [MicroPython using rshell](https://www.twilio.com/blog/programming-raspberry-pi-pico-microcontroller-micropython)

### Schematic Capture

* [Some suggested tools](https://linuxhint.com/best_circuit_design_tools/)
* [Xschem](https://xschem.sourceforge.io/stefan/index.html)
* [Fritzing](https://fritzing.org/download/)

