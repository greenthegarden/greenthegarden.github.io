---
layout: post
title: UPS NUT Server configuration
description: Configuration for a UPS NUT server
categories: UPS, NUT
---

Configuration for a UPS NUT server

# UPS NUT Server configuration

##  Network UPS tools

Based on instructions at

* http://wynandbooysen.com/raspberry-pi-ups-server-using-nut.html
* http://abakalidis.blogspot.com.au/2013/04/using-raspberry-pi-as-ups-server-with.html

## Installation

```
sudo apt-get install nut-client nut-server
```

Add nut group to pi user

```
sudo usermod -a -G nut pi
```

## Configuration

Note: for a [Synology](http://www.synology.com) NAS to access the nut server specific values need to be set!! See https://tellini.info/2014/09/connecting-a-synology-diskstation-to-a-nut-server/.

Set up the driver for the nut server by adding the following lines to the file `/etc/nut/ups.conf`. To support a Synology NAS the name must be `ups`.

```
[ups]
       driver = usbhid-ups
       port = auto
       desc = "APC Back-UPS ES 550"
```

Test the UPS driver

```
upsdrvctl start
```

Should see something similar to the following

```
Network UPS Tools - UPS driver controller 2.7.2
Network UPS Tools - Generic HID driver 0.38 (2.7.2)
USB communication driver 0.32
Using subdriver: APC HID 0.95
```

Configure `upsd` by adding the following to the file `/etc/nut/upsd.conf`, where `192.168.1.52` is the IP address of the NUT server.

```
LISTEN 127.0.0.1 3493
LISTEN 192.168.1.52 3493
```

Configure users by adding the following to the file `/etc/nut/upsd.users`.

```
[upsmon_local]
    password = pi
    upsmon master
[upsmon_remote]
    password = pi
    upsmon slave
# The following user is required for a Synology NAS
[monuser]
    password = secret
    upsmon slave
```

Specify the UPS for the UPS monitor daemon to monitor by adding the following lines to the file `/etc/nut/upsmon.conf`.

```
MONITOR ups@localhost 1 upsmon_local pi master
```

Open the `/etc/nut/nut.conf` file and change the value of Mode to netserver -- making sure that there are no spaces between each side of the = sign

Change from

```
MODE=none
```

to

```
MODE=netserver
```

To start server and client use

```
sudo service nut-server start
sudo service nut-client start
```

To test use

```
upsc ups
```

Example result

```
Init SSL without certificate database
battery.charge: 100
battery.charge.low: 10
battery.charge.warning: 50
battery.date: not set
battery.mfr.date: 2011/02/07
battery.runtime: 2535
battery.runtime.low: 120
battery.type: PbAc
battery.voltage: 13.6
battery.voltage.nominal: 12.0
device.mfr: APC
device.model: Back-UPS ES 550G
device.serial: 5B1106T47175
device.type: ups
driver.name: usbhid-ups
driver.parameter.pollfreq: 30
driver.parameter.pollinterval: 2
driver.parameter.port: auto
driver.version: 2.7.2
driver.version.data: APC HID 0.95
driver.version.internal: 0.38
input.sensitivity: medium
input.transfer.high: 266
input.transfer.low: 180
input.voltage: 244.0
input.voltage.nominal: 230
ups.beeper.status: enabled
ups.delay.shutdown: 20
ups.firmware: 870.O2 .I
ups.firmware.aux: O2
ups.load: 2
ups.mfr: APC
ups.mfr.date: 2011/02/07
ups.model: Back-UPS ES 550G
ups.productid: 0002
ups.serial: 5B1106T47175
ups.status: OL
ups.timer.reboot: 0
ups.timer.shutdown: -1
ups.vendorid: 051d
```

Open port to allow remote access, using

```
ssudo ufw allow from 192.168.1.0/24 to any port 3493
```

## openHAB2 configuration

For use with openHAB2 install the Network UPS Tools binding.

Add the following settings to file `/etc/openhab2/services/networkupstools.cfg`.

1. UPS device name: ups
2. UPS server hostname (optional): localhost
3. UPS server port (optional): 3493
4. UPS server login (optional): pi
5. UPS server pass (optional): pi
