---
layout: post
title: emonPi Installation
description: Building an emonPi disk
categories: emonPi, energy, monitoring, IoT
---

The modifications I used to build an emonPi disk.

I plan to utilise my [emonPi](https://openenergymonitor.org/emon/) as the basis of my home automation system. I have struggled with the read-only configuration of the standard build so decided to build from stratch. Not as difficult given instructions at https://github.com/openenergymonitor/emonpi/blob/master/docs/SD-card-build.md. I have started the build using NOOBS LITE Version 2.0.0, on October 15, 2016. However, there were some issues which are captured here:

* In Section 1, "Change Password", should there be a `sudo`?
* Ignored Section 2.
* In Section 3, "Using custom cmdline.txt", I did not use the `cmdline.txt` from the git repository, as the original `cmdline.txt` was slightly different to that documented. Instead, I edited the `cmdline.txt` file already installed by removing the `console=serial0,115200` (was not `ttyAMA0` as in instructions) and changing `elevator=deadline` to  `elevator=noop`.
* In Section 3, "Disable serial console boot", although I followed the commands as specified, I am sure they do not make sense given `ttyAMA0` was not in the original `cmdline.txt` file.
* In Section 5, the installation of MQTT I installed MQTT from the debian wheezy repository, using `sudo apt-get install mosquitto`.
* Before running command `sudo sh -c 'echo "extension=mosquitto.so" > /etc/php5/apache2/conf.d/20-mosquitto.ini'` needed to create directories `/etc/php5/apache2` and `/etc/php5/apache2/conf.d`.
* In Section 5, "Install emonHub (emon-pi) variant" need to create a directory `/home/pi/data` before running `emonhub/install` otherwise script fails.
* In Section 7, "emonPi specific settings" I did not move feed directories to RW partition, but did have to recompile phpredis.
* Ignored Section 7, "Move MYSQL database location".
* In Section 10, command `sudo ln -s /home/pi/emonpi/emonpi/rc.local_jessieminimal /etc/rc.local` should be `sudo ln -s /home/pi/emonpi/rc.local_jessieminimal /etc/rc.local`.
* Ignored Sections 14, 16-19.
* For Section 15, used instructions for Java 8 for openHAB installation.


See [openHAB install](../openhab-installation) for next steps.
