---
layout: post
title: emonpi build
description:
categories: emonpi, openhab2, radicale
---

# emonpi Build

I plan to utilise my [emonPi](https://openenergymonitor.org/emon/) as the basis of my home automation system which is based on [openHAB2](https://openhab.org).

## Base

### Disk Image

The base disk image I have used is [emonSD-07Nov16 BETA](https://community.openenergymonitor.org/t/emonsd-07nov16-release-tada/2137) which I flashed to a 16MB DS card using []().

### Expand file system

Use the utility provided to expand the file system to fit storage capacity

```
emonSDExpand
```

### Updates

Apply updates as standard

```
sudo apt-get update
sudo apt-get upgrade
sudo apt dist-upgrade
```

### Switch off services

Switch of services not being utilised

```
sudo systemctl disable openhab.service
sudo systemctl disable nodered.service
```


### Install pip3

pip3 is required to install additional packages for Radicale.

Install using

```
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
```

### Install Radicale

Install from github

```
cd
git clone https://github.com/Kozea/Radicale.git
```

Install required modules

```
pip3 install --user vobject --upgrade
```

Make radicale user and group

```
sudo adduser --system --no-create-home --group radicale
sudo usermod -a -G radicale pi
```

Copy the configuration files to /etc/radicale/ and set ownership and permissions.

```
sudo mkdir -p /etc/radicale
cp Radicale/config /etc/radicale/
cp Radicale/logging /etc/radicale/
sudo chown -R radicale:radicale /etc/radicale
sudo chmod -R g+rw /etc/radicale
```

Create logging file and set ownership.

```
sudo touch data/log/radicale
sudo chown radicale:radicale data/log/radicale
sudo chmod -R g+rw data/log/radicale
```

Make a directory to store collections

```
sudo mkdir -p /home/pi/data/radicale/collections
sudo chown -R radicale:radicale /home/pi/data/radicale
sudo chmod -R g+rw /home/pi/data/radicale
```

Make the following changes to the file `/etc/radicale`

To open the port to enable access to the server use

```
sudo ufw allow from 192.168.1.0/24 to any port 5232
```

Open a browser to <http://emonpi:5232> and login with `pi` credentials.

Create a calender called `openhab-events`.

Add entry to crontab to start Radicale on reboot using

```
crontab -e
@reboot python3 /home/pi/Radicale/radicale.py
```

### Configure nut server

See instructions at  to install and configure nut server.

Add pi user nut group

```
sudo usermod -a -G nut pi
```

### Install openHAB2

Install based on instructions at <http://docs.openhab.org/installation/linux.html#package-repository-installation>

```
wget -qO - 'https://bintray.com/user/downloadSubjectPublicKey?username=openhab' | sudo apt-key add -
sudo apt-get install apt-transport-https
```

Install the official release

```
echo 'deb https://dl.bintray.com/openhab/apt-repo2 stable main' | sudo tee /etc/apt/sources.list.d/openhab2.list
sudo apt-get update
sudo apt-get install openhab2
```

Add pi user openhab group

```
sudo usermod -a -G openhab pi
```

Load configuration from git repository

```
cd /etc
sudo rm -rf openhab2
sudo git clone git@192.168.1.51:/home/git/repositories/openhab2/config/openhab2.git
sudo chown -R openhab:openhab openhab2
sudo chmod -R g+rw openhab2
```




The following changes are required to support the read-only filesystem.
