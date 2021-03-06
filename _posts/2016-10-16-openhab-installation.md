---
layout: post
title: openHAB Installation
description: Installing and configuring openHAB
categories: openHAB, automation, IoT
---

I run [openHAB](http://www.openhab.org/) on a [Raspberry Pi](https://www.raspberrypi.org/) as the basis of my home automation using the following configuration.

## References

I have found the following references to be particularly useful:

  * [Getting Started with OpenHAB Home Automation on Raspberry Pi](http://www.makeuseof.com/tag/getting-started-openhab-home-automation-raspberry-pi/)
  * [OpenHAB Beginner’s Guide Part 2: ZWave, MQTT, Rules and Charting](http://www.makeuseof.com/tag/openhab-beginners-guide-part-2-zwave-mqtt-rules-charting/)

## Installation of runtime

Add repository information

```
wget -qO - 'https://bintray.com/user/downloadSubjectPublicKey?username=openhab' |sudo apt-key add -
echo "deb http://dl.bintray.com/openhab/apt-repo stable main" | sudo tee /etc/apt/sources.list.d/openhab.list
sudo apt-get update
```

Install openHAB runtime

```
sudo apt-get install openhab-runtime
```

Statement at end of installation to automatically start

```
### NOT starting on installation, please execute the following statements to configure openHAB to start automatically using systemd
 sudo /bin/systemctl daemon-reload
 sudo /bin/systemctl enable openhab.service
### You can start openhab by executing
 sudo /bin/systemctl start openhab.service
```

To restart use

```
sudo /bin/systemctl restart openhab.service
```

Set group and owner to openhab (from root)

```
sudo chown -hR openhab:openhab /etc/openhab
sudo chown -hR openhab:openhab /usr/share/openhab
```

Add pi user to openhab group

```
sudo usermod -a -G openhab pi
```

## Installation of bindings

Installation of bindings uses the following form

```
sudo apt-get install openhab-addon-${addon-type}-${addon-name}
```

To find the name of binding use

```
sudo apt-cache search openhab
```

# Other Packages

Other packages are required to be installed to support the addition openHAB capabilities I utilise to support my openHAB implementation.

## Samba

[Samba](https://www.samba.org/) is utilised to make it easier to edit openHAB configurations via the [openHAB designer](http://www.openhab.org/getting-started/downloads.html) application on external computers.

### Installation

```
sudo apt-get install samba samba-common-bin
```

### Configuration

Edit the file `/etc/samba/smb.conf` to make the following changes.

#### Enable wins support

Change `# wins support = no` to `wins support = yes`.

#### Enable symbolic link support

Add the following lines, under `[global]`

```
follow symlinks = yes
wide links = yes
```

Comment out (place `;` at start of line) all references to printers.

#### Share openhab folders

Add the following lines to the end of the file to share the specified directories.

Share pi home directory:

```
[pi Home]
comment= pi Home
path=/home/pi
valid users = @pi
browseable=Yes
writeable=Yes
only guest=no
create mask=0777
directory mask=0777
public=no
```

Share openhab related directories:

```
[OpenHAB Home]
comment= openHAB Home
path=/usr/share/openhab
valid users = @pi @openhab
browseable=Yes
writeable=Yes
only guest=no
create mask=0777
directory mask=0777
public=no

[OpenHAB Config]
comment= openHAB Site Config
valid users = @pi @openhab
path=/etc/openhab
browseable=Yes
writeable=Yes
only guest=no
create mask=0777
directory mask=0777
public=no
```

Set passwords for pi and openhab user

```
sudo smbpasswd -a pi
sudo smbpasswd -a openhab
```

Restart samba

```
sudo /bin/systemctl restart smbd.service
```

#### Test shared drive

On Ubuntu use `Files -> File -> Connect to Server...` and the address `smb://192.168.2.90`. Leave domain as `WORKGROUP` in dialog window.

On Mac use `Finder -> Go -> Connect to Server` and the address `smb://openhab@raspberrypi.local`.


## MQTT

I make significant use of [MQTT](http://mqtt.org/) in order to share data between the various components of my home automation systems. openHAB supports MQTT via the [MQTT binding](https://github.com/openhab/openhab/wiki/MQTT-Binding).

### Binding Installation

```
sudo apt-get install openhab-addon-binding-mqtt
```

### Mosquitto

In order to utilise MQTT a broker is required, for which I use [Mosquitto](https://mosquitto.org/).

#### Installation

```
sudo apt-get install mosquitto
```

### Configuration

The configuration of the `<broker>` needs to be applied in the section `MQTT Transport` of the file `/etc/openhab/configurations/openhab.cfg` to configure the MQTT binding.

For example, if installing on emonpi:

```
mqtt:emonpi.url=tcp://localhost:1883
mqtt:emonpi.user=emonpi
mqtt:emonpi.pwd=emonpimqtt2016
mqtt:emonpi.qos=2
mqtt:emonpi.retain=false
```

### Use

Make sure whatever has been defined for `<broker>` in the openHAB configuration file is reflected in the `items` files.

## CalDAV events

openHAB supports the triggering of parameters by events defined in CalDAV calendars via the [CalDAV binding](https://github.com/openhab/openhab/wiki/CalDAV). In addition to be able to record events within calendars via the CalDAV persistence binding.

### Binding Installation

```
sudo apt-get install openhab-addon-binding-caldav-command
sudo apt-get install openhab-addon-binding-caldav-personal
sudo apt-get install openhab-addon-io-caldav
sudo apt-get install openhab-addon-persistence-caldav

```

### Radicale

In order to utilise the CalDAV bindings a compible CAlDAV sercver is required for which I have utilised [Radicale Project](http://radicale.org/).

#### Installation

```
sudo apt-get install radicale
```

#### Configuration

Guides to configure radicale:

* http://jonathantutorial.blogspot.com.au/2014/10/how-to-set-up-radicale.html
* https://evilshit.wordpress.com/2013/11/19/how-to-install-a-caldav-and-carddav-server-using-radicale/

##### Run as daemon

Open the file `/etc/radicale/config` and change `#daemon = False` to `daemon = True`.

To have radicale automatically start open the file `/etc/default/radicale` and uncomment the line `#ENABLE_RADICALE=yes`.

To start radicale use

```
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable radicale.service
sudo /bin/systemctl start radicale.service
```

Not sure why the above does not work, but starts as expected on a reboot.

Test connection using opening the address `http://emonpi:5232` in a browser. If all is good, should show `Radicale works!`.

Add pi user to radicale group using

```
usermod -a -G radicale pi
```

### Use

#### Calender creation

Need to create two calenders
  - openhab, as http://emonpi:5232/openhab/openhab.ics/
  - history, as http://emonpi:5232/openhab/history.ics/

## MySQL

[MySQL](http://www.mysql.com/) is utilised as a support the capturing of data in order to enable post-processing.

### Installation

```
sudo apt-get install mysql-server python-mysqldb
```

Will be required to set a root password during the installation.

### Configuration

Connect to mySQL server using

```
mysql -u root -p
```

Create a new database for openHAB

```
CREATE DATABASE openhab;
```

Create an 'openhab' user with password 'openhab'

```
CREATE USER 'openhab'@'localhost' IDENTIFIED BY 'openhab';
```

Set permissions for 'openhab' user to access 'openhab' database

```
GRANT ALL PRIVILEGES ON openhab . * TO 'openhab'@'localhost';
```

Configure openHAB to use mysql persistence.

##  Network UPS tools

Based on instructions at http://abakalidis.blogspot.com.au/2013/04/using-raspberry-pi-as-ups-server-with.html

```
sudo apt-get install nut-client nut-server
```

Add nut group to pi user

```
sudo usermod -a -G nut pi
```

Note: for a Synology NAS to access the nut server specific values need to be set!! See https://tellini.info/2014/09/connecting-a-synology-diskstation-to-a-nut-server/

Set up the driver for the nut server by adding the following lines to the file `/etc/nut/ups.conf`

```
[ups]
  driver = usbhid-ups
  port = auto
  desc = "APC ES550"
```

Test the UPS driver

```
upsdrvctl start
```

Configure upsd by adding the following to the file `/etc/nut/upsd.conf`, where 192.168.1.55 is the IP address of the PI.

```
LISTEN 127.0.0.1 3493
LISTEN 192.168.1.55 3493
```

Configure users by adding the following to the file `/etc/nut/upsd.users`:

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

Specify the UPS for the UPS monitor daeon to monitor by adding the following lines to the file `/etc/nut/upsmon.conf`:

```
MONITOR ups@localhost 1 upsmon_local pi master
```

Open the file `/etc/nut/nut.conf` and change the value of Mode to netserver -- making sure that there are no spaces between each side of the = sign

Change from:

```
Mode=none
```

to

```
Mode=netserver
```

To start server and client use

```
service nut-server start
service nut-client start
```

To test use

```
upsc ups
```

Example result:

```
battery.charge: 100
battery.charge.low: 10
battery.charge.warning: 50
battery.date: not set
battery.mfr.date: 2011/02/07
battery.runtime: 1567
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
ups.load: 11
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

## Sigar Installation

To use the SystemInfo Binding on a PI

Instructions based on information at  https://github.com/openhab/openhab/wiki/Systeminfo-Binding

```
wget https://groups.google.com/group/openhab/attach/ab7030271be23f05/sigar-raspbian.zip?part=0.1 -O ~/sigar-raspbian.zip
```

Unzip archive:

```
unzip ~/sigar-raspbian.zip -d ~
```

Create a lib folder first for apt-get installations:

```
sudo mkdir /usr/share/openhab/lib
```

Then copy the downloaded files to the new lib folder:

```
sudo cp ~/sigar-raspbian/lib/* /usr/share/openhab/lib
```




Use the following to install bindings for
http, ntp, weather, persistence-logging, persistence-rrd4j, persistence-mysql, mqtt, systeminfo, astro, networkupstools, caldav, dropbox

```
sudo apt-get install openhab-addon-binding-astro
sudo apt-get install openhab-addon-binding-http

sudo apt-get install openhab-addon-binding-networkhealth
sudo apt-get install openhab-addon-binding-networkupstools
sudo apt-get install openhab-addon-binding-ntp
sudo apt-get install openhab-addon-binding-systeminfo
sudo apt-get install openhab-addon-binding-weather
sudo apt-get install openhab-addon-io-dropbox
sudo apt-get install openhab-addon-persistence-logging
sudo apt-get install openhab-addon-persistence-mysql
sudo apt-get install openhab-addon-persistence-rrd4j
```






### UPS Network

Information available:

```
Storology> upsc ups
```

Example output:

```
battery.charge: 100
battery.charge.low: 10
battery.charge.warning: 50
battery.date: not set
battery.mfr.date: 2011/02/07
battery.runtime: 3270
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
driver.parameter.pollinterval: 5
driver.parameter.port: auto
driver.version: SDS5-2-2015Q1branch-5619-150904
driver.version.data: APC HID 0.95
driver.version.internal: 0.38
input.sensitivity: medium
input.transfer.high: 266
input.transfer.low: 180
input.voltage: 240.0
input.voltage.nominal: 230
ups.beeper.status: enabled
ups.delay.shutdown: 20
ups.firmware: 870.O2 .I
ups.firmware.aux: O2
ups.load: 0
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

### Image Refresh

Refreshing an image periodically (every second)

Define the following in the sitemap:

```
Remote image:
Image url="http://192.168.1.55/jpg/image.jpg" refresh=1000

Locale image:
Image url="/jpg/image.jpg" refresh=1000
```
The image "jpg/image.jpg" needs to be located in `$OPENHAB_ROOT/webapps/` folder.

### Dropbox Integration

See https://github.com/openhab/openhab/wiki/Dropbox-IO
