---
layout: post
title: Raspberry Pi Bridge
---

A useful capability is to use a Raspberry Pi as a WiFi to Ethernet bridge in order to connect wired devices when a router is not conveniently located.

# Requirements

* Raspberry Pi model with an ethernet port ([Raspberry Pi 3 Model B](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/), [Raspberry Pi 2 Model B](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/), or even [Raspberry Pi 1 Model B/B+](https://www.raspberrypi.org/products/model-b-plus/)).
* A WiFi capability for the Raspberry Pi, either:
  * included with the Raspberry Pi 3 Model B, or
  * a USB WiFi dongle, such the Raspberry Pi [USB WiFi dongle](https://www.raspberrypi.org/products/usb-wifi-dongle/), or
  * a HAT, such as the [Redbear IoT pHAT](https://redbear.cc/product/rpi/iot-phat.html).
* A configured WiFi router.
* An SD card to install the software on with a minimum capacity of 2GB.

# Initial Software Build

Install Raspbian Jessie Lite using either [NOOBS](https://www.raspberrypi.org/downloads/noobs/) or a [disk image](https://www.raspberrypi.org/downloads/raspbian/).

# Sources

* http://www.glennklockwood.com/sysadmin-howtos/rpi-wifi-island.html
* http://elinux.org/RPI-Wireless-Hotspot

# Additional Packages

Install the following additional packages:

* [dhcp](https://en.wikipedia.org/wiki/DHCPD)
* [Hostapd](https://en.wikipedia.org/wiki/Hostapd)

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install hostapd udhcpd
```
