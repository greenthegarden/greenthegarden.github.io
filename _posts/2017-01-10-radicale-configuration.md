---
layout: post
title: Radicale configuration
description: Configuration for Radicale, a CalDAV server.
categories: UPS, NUT
---

Configuration I use for Radicale, a CalDAV server.

# Radicale

[Radicale Project](http://radicale.org/) provides a CalDAV and CardDAV server capability which I utilise in order to maintain local calenders which everyone in the family can access and hopefully keep in sync with things. I have installed Radicale on a Raspberry Pi v2.

## Installation from package manager

The version of Radicale installed using apt is 0.9 as of Feb 2017.

```
sudo apt-get install radicale
```

Create a radicale user and group which is used to run Radicale. The pi user can be added to the radicale group using

```
sudo usermod -a -G radicale pi
```

### Default File and directory locations

Files and Directories:
* /etc/default/radicale (file) - startup settings
* /etc/radicale/config (file) - configuration file
* /etc/radicale/users (file) - your htpasswd file storing your usernames and passwords
* /var/lib/radicale (directory) - radicale library, find the iCalendar files in collections directory here
* /etc/radicale/ssl (directory) - SSL files
* /var/log/radicale (directory) - log files

## Installation via pip

Installing via pip will install version 1.1.1, as of Feb 2017.

```
sudo pip install radicale
```

## Installation from git

I have elected to install Radicale directly via git in the home directory of pi.

```
cd /home/pi
git clone git://github.com/Kozea/Radicale.git
```

## Configuration

Guides to configure radicale:

* http://jonathantutorial.blogspot.com.au/2014/10/how-to-set-up-radicale.html
* https://evilshit.wordpress.com/2013/11/19/how-to-install-a-caldav-and-carddav-server-using-radicale/
* http://christian.kuelker.info/doc/radicale/calendars-todo-lists-and-contacts-with-radicale.html
* http://gedakc.users.sourceforge.net/display-doc.php?name=android-davdroid-radicale-setup

### Configuration file

#### apt-get

If installed via apt-get, configuration file is at `/etc/radicale/config`.

#### git

If installed via git, the default configuration file `config` is in the git directory. By default, it is required to be copied to `~/.config/radicale/config`, using

```
cd /home/pi
mkdir -p .config/radicale
cp Radicale/config .config/radicale/
```

### Run as daemon

Open the config file and change `#daemon = False` to `daemon = True`.

#### apt-get

To have radicale automatically start open the file `/etc/default/radicale` and uncomment the line `#ENABLE_RADICALE=yes`.

#### git

Create a cron job to start automatically.

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

### Starting

#### apt-get

Use

```
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable radicale.service
sudo /bin/systemctl start radicale.service
```

Not sure why the above does not work, but starts as expected on a reboot.

#### git

Use

```
cd /home/pi
Radicale/radicale.py
```

### Open port

To open the port to enable access to the server use

```
sudo ufw allow from 192.168.1.0/24 to any port 5232
```

### Test connection

Test connection using opening the address `http://192.168.1.XXX:5232` in a browser. If all is good, should show `Radicale works!`.

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
  4. Base URL: http://192.168.1.51:5232
  5. User name: openhab
  6. Password: openhab
  7. Create account: openHAB@radicale

In step 7, need to ensure account name is in the form of an email address, otherwise accounts with not work within Android.

When creating calendars and address books with DAVdroid the associated folders have randomly generated names, which makes them hard to distinguish from each other.

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
