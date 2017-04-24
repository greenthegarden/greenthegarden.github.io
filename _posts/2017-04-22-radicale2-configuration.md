---
layout: post
title: Radicale2 configuration
description: Configuration for Radicale2, a CalDAV server.
categories: Radicale, CalDAV, CardDAV
---

Configuration I use for Radicale2, a CalDAV server.

# Radicale

[Radicale Project](http://radicale.org/) provides a CalDAV and CardDAV server capability which I utilise in order to maintain local calenders which everyone in the family can access and hopefully keep in sync with things. I have installed Radicale on a Raspberry Pi v2.

Files and Directories:
*   /etc/default/radicale (file) - startup settings
*   /etc/radicale/config (file) - configuration file
*   /etc/radicale/users (file) - your htpasswd file storing your usernames and passwords
*   /var/lib/radicale (directory) - radicale library, find the iCalendar files in collections directory here
*   /etc/radicale/ssl (directory) - SSL files
*   /var/log/radicale (directory) - log files

## Installation from git

I have elected to install Radicale directly via git in the home directory of pi.

```
cd /home/pi
git clone git://github.com/Kozea/Radicale.git
```

### Radicale user and group

Make radicale user and group

```
sudo adduser --system --no-create-home --group radicale
sudo usermod -a -G radicale pi
```


### Configuration

Move the configuration files to /etc/radicale/ and set ownership and permissions.

```
sudo mkdir -p /etc/radicale
cp Radicale/config /etc/radicale/
cp Radicale/logging /etc/radicale/
sudo chown -R radicale:radicale /etc/radicale
sudo chmod -R g+rw /etc/radicale
```

### Logging

Create logging file and set ownership.

```
sudo touch /var/log/radicale
sudo chown radicale:radicale /var/log/radicale
sudo chmod -R g+rw /var/log/radicale
```

### Create collection directory

Create collection directory and set ownership.

On a standard pi, use


```
sudo mkdir -p /var/lib/radicale/collections
sudo chown -R radicale:radicale /var/lib/radicale
sudo chmod -R g+rw /var/lib/radicale
```
On emonpi, use

```
sudo mkdir -p /home/pi/data/radicale/collections
sudo chown -R radicale:radicale/home/pi/data/radicale
sudo chmod -R g+rw /home/pi/data/radicale
```


## Configuration

### Guides

Guides to configure radicale:

*   http://jonathantutorial.blogspot.com.au/2014/10/how-to-set-up-radicale.html
*   https://evilshit.wordpress.com/2013/11/19/how-to-install-a-caldav-and-carddav-server-using-radicale/
*   http://christian.kuelker.info/doc/radicale/calendars-todo-lists-and-contacts-with-radicale.html
*   http://gedakc.users.sourceforge.net/display-doc.php?name=android-davdroid-radicale-setup

### Run as daemon

Open the config file and change `#daemon = False` to `daemon = True`.

### Configure passwords

To create users and passwords using htpasswd, edit the radicale configuration file and under `[auth]` change `#type = None` to `type = htpasswd`. Install the `apache2-utils` package if not already installed, using

```
sudo apt-get install apache2-utils
```

To create the password file and a new user/password combination use,

```
sudo mkdir /etc/radicale
sudo htpasswd -cs /etc/radicale/users <user>
```

Note the the `-c` argument is used to create the file (in this case /etc/radicale/users). If the file already exists it will be overwrittem.

To create additional users, users

```
sudo htpasswd -s /etc/radicale/users <user2>
```

## Open port

To open the port to enable access to the server use

```
sudo ufw allow from 192.168.1.0/24 to any port 5232
```

## Starting

### apt-get

Use

```
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable radicale.service
sudo /bin/systemctl start radicale.service
```

Not sure why the above does not work, but starts as expected on a reboot.

### git

Use

```
cd /home/pi
Radicale/radicale.py
```

## Test connection

Test connection using opening the address `http://192.168.1.XXX:5232` in a browser. If all is good, should show `Radicale works!`.

## Usages

### Calender creation

#### Android with davdroid

On Android, I use the package [DAVdroid](https://davdroid.bitfire.at/) for provide CalDAV (and CardDAV) support. To connect to calenders use the following steps:

1.  Add account
2.  Select `Login with URL and user name`
3.  Enter following settings:
4.  Base URL: http://192.168.1.51:5232
5.  User name: <user>
6.  Password: <password>
7.  Create account: <user>@radicale.piology
8.  Cantact group method: Groups are per-contact categories

In step 7, need to ensure account name is in the form of an email address, otherwise accounts with not work within Android.

When creating calendars and address books with DAVdroid the associated folders have randomly generated names, which makes them hard to distinguish from each other.

#### Thunderbird Lightning plug-in

Calenders can be created via a CalDAV client. On desktop machines I use [Thunderbird](https://www.mozilla.org/en-US/thunderbird/) which in later versions includes the `Lightning` calender plug-in.

## openHAB support

CalDAV support is provided in [openHAB](http://openhab.org) via the [CalDAV Binding](https://github.com/openhab/openhab1-addons/wiki/CalDAV). I use the binding to schedule events on and off via calendar entries.

### Calendars

Need to create two calenders:

*   openHAB Events, as http://emonpi:5232/radicale/openhab/openhabevents.ics/

*   openHAB History, as http://emonpi:5232/radicale/openhab/openhabhistory.ics/

To create calenders use to the following settings:

1.  New Calender ...
2.  On the Network
3.  Format: CalDAV
4.  Location: As defined above

### Configuration

Add the following lines to the file caldavio.cfg to allow access the calenders:

```
caldavio:openhabevents:url=localhost:5323/radicale/openhab/openhabevents.ics
caldavio:openhabevents:username=openhab
caldavio:openhabevents:password=openhab
caldavio:openhabevents:reloadInterval=2

caldavio:openhabhistory:url=localhost:5323/radicale/openhab/openhabhistory.ics
caldavio:openhabhistory:username=openhab
caldavio:openhabhistory:password=openhab
```
