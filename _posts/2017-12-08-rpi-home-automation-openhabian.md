---
layout: post
title: Configuring openHABian
description:
categories: RPi, home automation, docker, openHAB, radicale,
---

# Home Automation on RPi with openHABian

After stability problems with Docker give openHABian a go.

## Install openHABian

Follow instructions at http://docs.openhab.org/installation/openhabian.html.

## Install Radicale

### Install pip3

pip3 is required to install Radicale from PyPI

Install using

```
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
```

Follow installation instructions at http://radicale.org/tutorial/

```
sudo python3 -m pip install --upgrade radicale
```

Follow instructions at http://radicale.org/setup/ under "Linux with systemd system-wide" to confiugre a service

```
sudo useradd --system --home-dir / --shell /sbin/nologin radicale
sudo usermod -a -G radicale ${USER}
sudo mkdir -p /var/lib/radicale/collections
exit
sudo chown -R radicale:radicale /var/lib/radicale/collections
sudo chmod -R o= /var/lib/radicale/collections
sudo vi /etc/systemd/system/radicale.service
```

Create configuration file

```
sudo mkdir /etc/radicale
sudo vi /etc/radicale/config
```

```
[server]
# Bind all addresses
hosts = 192.168.1.2:5232

[auth]
type = none

[storage]
filesystem_folder = /var/lib/radicale/collections
```

```
sudo chown -R radicale:radicale /etc/radicale
sudo chmod g+w /etc/radicale/config
```

Run service

```
sudo systemctl enable radicale
sudo systemctl start radicale
sudo systemctl status radicale
```

Check running at http://openhabianpi:5232
