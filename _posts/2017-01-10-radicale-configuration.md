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
* http://christian.kuelker.info/doc/radicale/calendars-todo-lists-and-contacts-with-radicale.html

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


### Configure passwords

### Starting

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

* openHAB Events, as http://emonpi:5232/radicale/openhab/openhabevents.ics/

* openHAB History, as http://emonpi:5232/radicale/openhab/openhabhistory.ics/

To create calenders use to the following settings:

1. New Calender ...
2. On the Network
3. Format: CalDAV
4. Location: As defined above


On Android, I use the package [DAVdroid](https://davdroid.bitfire.at/) for provide CalDAV (and CardDAV) support. To connect to calenders use the following steps:

1. Add account
2. Select `Login with URL and user name`
3. Enter following settings:
  4. Base URL: http://192.168.1.52:5232/radicale/openhab/
  5. User name: openhab
  6. Password: openhab
  7. Create account: openHAB

## openHAB configuration

### caldavio.cfg

Add the following lines to access the calenders:

```
caldavio:openhabevents:url=localhost:5323/radicale/openhab/openhabevents.ics
caldavio:openhabevents:username=openhab
caldavio:openhabevents:password=openhab
caldavio:openhabevents:reloadInterval=2

caldavio:openhabhistory:url=localhost:5323/radicale/openhab/openhabhistory.ics
caldavio:openhabhistory:username=openhab
caldavio:openhabhistory:password=openhab
```
