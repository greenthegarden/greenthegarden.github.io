---
layout: post
title: openHAB 1 to openHAB 2
description: Upgradeing from openHAB 1 to openHAB 2
categories: openHAB, automation, IoT
---

As mentioned in the post [openHAB install](../openhab-installation) I have utilised [openHAB](http://openhab.org) as the centre of my home automation system. However, given the fact I was building a new installation, based around an [emonPi](https://guide.openenergymonitor.org/), and more mature versions of openHAB 2 are now available, I thought I may as well take the plunge and upgrade. The following details my experience of the upgrade.

# Starting point

The instructions for setting up openHAB on a Raspberry Pi are located at http://docs.openhab.org/installation/rasppi.html, which I used as a basis for the install.

# Stopping openHAB 1

For a information about services, see https://docs.fedoraproject.org/en-US/Fedora/15/html/Deployment_Guide/ch-Services_and_Daemons.html.

To check whether the openHAB service (openHAB 1) is running, use,

```
sudo systemctl is-active openhab.service
```

or, for more details,

```
sudo systemctl status openhab.service
```


To stop the existing openHAB 1 service, use,

```
sudo systemctl stop openhab.service
```

To disable the service, and prevent it from starting at boot time, use

```
sudo systemctl disable openhab.service
```

# Configure samba

Enable sharing of openHAB2 directories, by adding the following to the file `/etc/samba/smb.conf`

```
[OpenHAB2 Home]
comment= openHAB2 Home
path=/usr/share/openhab2
valid users = @pi @openhab
browseable=Yes
writeable=Yes
only guest=no
create mask=0777
directory mask=0777
public=no

[OpenHAB2 Config]
comment= openHAB2 Site Config
path=/etc/openhab2
valid users = @pi @openhab
browseable=Yes
writeable=Yes
only guest=no
create mask=0777
directory mask=0777
public=no
```

Enable samba ports through firewall

```
sudo ufw allow from 192.168.1.0/24 to any port 137 proto udp
sudo ufw allow from 192.168.1.0/24 to any port 138 proto udp
sudo ufw allow from 192.168.1.0/24 to any port 139 proto tcp
sudo ufw allow from 192.168.1.0/24 to any port 445 proto tcp
```

# Install Eclipse SmartHome Designer

See instructions at http://docs.openhab.org/installation/designer.html.

Installed using

```
sudo unzip -d /opt/designer eclipsesmarthome-incubation-0.8.0-designer-linux64.zip
```

# Start openHAB2 service

```
sudo systemctl start openhab2.service
sudo systemctl status openhab2.service

sudo systemctl daemon-reload
sudo systemctl enable openhab2.service
```

# Congfiguration

Open brower and go to http://emonpi:8080/.

Select `Paper UI`.

Select `Configuration > System` and enter `en` for language and `AU` for region.

Select `Extensions` and install the following bindings

* Astro Binding
* MQTT Binding
* Network Binding
* NTP Binding
* System Info Binding
* Weather Binding

Select `Configurations > Things` and select a Thing to edit it.

Play around. Bindings can be configured and added automatically. WOW!!

With a change to the channel setting in the item files, and removal of import statements in rule files all openHAB 1 configuration files have worked.

Non-version 2 bindings need to be configured via configuration files installed in the `services` directory. So much more convenient than the single huge configuration file used by openHAB 1.

# Persistence

To set up persistence, see

* https://community.openhab.org/t/oh2-and-rrd4j/14501/19, and
* https://community.openhab.org/t/openhab2-mysql-persistence-setup/15829

# my.openHAB

Get uuid and secret from `/var/lib/openhab2/uuid` and `/var/lib/openhab2/myopenhab/secret`, respectively.
