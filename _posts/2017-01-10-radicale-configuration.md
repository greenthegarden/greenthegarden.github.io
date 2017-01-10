---
layout: post
title: Radicale configuration
description: Configuration for Radicale, a CalDAV server.
categories: UPS, NUT
---

Configuration I use for Radicale, a CalDAV server.

# Radicale

[Radicale Project](http://radicale.org/) provides a CalDAV server capability which I utilise in order to maintain local calenders which everyone in the family can access and hopefully keep in sync with things. I have installed Radicale on a Raspberry Pi v2.

## Installation

```
sudo apt-get install radicale
```

Add pi user to radicale group using

```
sudo usermod -a -G radicale pi
```

## Configuration

Guides to configure radicale:

* http://jonathantutorial.blogspot.com.au/2014/10/how-to-set-up-radicale.html
* https://evilshit.wordpress.com/2013/11/19/how-to-install-a-caldav-and-carddav-server-using-radicale/

### Default File and directory locations

Files and Directories:
* /etc/default/radicale (file) - startup settings
* /etc/radicale/config (file) - configuration file
* /etc/radicale/users (file) - your htpasswd file storing your usernames and passwords
* /var/lib/radicale (directory) - radicale library, find the iCalendar files in collections directory here
* /etc/radicale/ssl (directory) - SSL files
* /var/log/radicale (directory) - log files

### Run as daemon

Open the file `/etc/radicale/config` and change `#daemon = False` to `daemon = True`.

To have radicale automatically start open the file `/etc/default/radicale` and uncomment the line `#ENABLE_RADICALE=yes`.

To start radicale use

```
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable radicale.service
sudo /bin/systemctl start radicale.service
```

Not sure why the above does not work, but starts as expected on a reboot.

### Open port

To open the port to enable access to the server use

```
sudo ufw allow from 192.168.1.0/24 to any port 5232
```

### Test connection

Test connection using opening the address `http://emonpi:5232` in a browser. If all is good, should show `Radicale works!`.

## Use

### Calender creation

Calenders can be created via a CalDAV client. On desktop machines I use [Thunderbird](https://www.mozilla.org/en-US/thunderbird/) which in later versions includes the `Lightning` calender plug-in.

Need to create two calenders:
* openHAB Events, as http://emonpi:5232/openhab/openhab.ics/
* openHAB History, as http://emonpi:5232/openhab/history.ics/

On Android, I use the package [DAVdroid](https://davdroid.bitfire.at/) for provide CalDAV (and CardDAV) support. To connect to calenders use the following steps:

1. Add account
2. Select `Login with URL and user name`
3. Enter following settings:
  4. Base URL: http://192.168.1.52:5232
  5. User name: pi
  6. Password: emonpi2016
  7. Create account: openHAB
