---
layout: post
title: Thingsboard RPi3 Install
description:
categories: RPi, home automation, docker, openHAB, radicale,
---

## Install Raspbian

Download latest version of Raspbian Lite from https://www.raspberrypi.org/downloads/raspbian/.

See the following to enable ssh https://www.raspberrypi.org/documentation/remote-access/ssh/README.md.

To find IP address see https://www.raspberrypi.org/documentation/remote-access/ip-address.md.

## Configure RPi

TO fix the local issue edit the file '/etc/ssh/sshd_config' and comment out the line 'AcceptEnv LANG LC_*'.

Configure the following items using

```
sudo rasp-config
```

- expand file system
- hostname: set to <hostname>
- timezone: set to Adelaide
- GPU memory: set to 16MB


## Update RPi

Update base OS using

``` bash
sudo apt-get update
sudo apt-get upgrade
```

Reboot to finish upgrade using

``` bash
sudo reboot
```

# Install Zulu OpenJDK

See http://zulu.org/

``` bash
sudo apt-get install dirmngr
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0x219BD9C9
sudo sh -c "echo 'deb http://repos.azulsystems.com/debian stable main' > /etc/apt/sources.list.d/zulu.list"
sudo apt-get update
sudo apt-get install zulu-embedded-8
```

# Install ThingsBoard

See https://thingsboard.io/docs/user-guide/install/rpi/

``` bash
# Download the package
wget https://github.com/thingsboard/thingsboard/releases/download/v1.3.1/thingsboard-1.3.1.deb
# Install ThingsBoard as a service
sudo dpkg -i thingsboard-1.3.1.deb
# Update ThingsBoard memory usage and restrict it to 150MB in /etc/thingsboard/conf/thingsboard.conf
export JAVA_OPTS="$JAVA_OPTS -Dplatform=rpi -Xms256M -Xmx256M"
# --loadDemo option will load demo data: users, devices, assets, rules, widgets.
sudo /usr/share/thingsboard/bin/install/install.sh --loadDemo
# start service
sudo service thingsboard start
```

Open Web UI using the following link http://localhost:8080/

First login use:

*   Username: tenant@thingsboard.org
*   Password: tenant
