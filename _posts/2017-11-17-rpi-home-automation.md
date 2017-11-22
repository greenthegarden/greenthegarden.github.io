---
layout: post
title: Configuring a RPi for Home Automation
description:
categories: RPi, home automation, docker, openHAB, radicale,
---

# Home Automation on RPi

## Install Raspbian

Download latest version of Raspbian Lite from https://www.raspberrypi.org/downloads/raspbian/.

See the following to enable ssh https://www.raspberrypi.org/documentation/remote-access/ssh/README.md.

To find IP address see https://www.raspberrypi.org/documentation/remote-access/ip-address.md.

## Configure RPi

Configure the following items using

```
sudo rasp-config
```

- expand file system
- hostname: set to openhab
- timezone: set to Adelaide
- GPU memory: set to 16MB

Configure cgroup memory by adding `cgroup_memory=1` using

```
sudo vi /boot/cmdline.txt
```


## Update RPi

Update base OS using

```
sudo apt-get update
sudo apt-get upgrade
```

<!-- ```
apt-get -y install cgroupfs-mount
``` -->

Reboot to finish upgrade using

```
sudo reboot
```

## Git

Install using

```
sudo apt-get install git
```

Configure with

```
git config --global user.email "greenthegarden@gmail.com"
git config --global user.name "Philip Cutler"
```

## Install docker

See
- https://www.raspberrypi.org/blog/docker-comes-to-raspberry-pi/
- https://blog.alexellis.io/getting-started-with-docker-on-raspberry-pi/

Run the following

```
curl -sSL https://get.docker.com | sh
```

Run the following to add pi user to docker group and configure docker to start automatically

```
sudo usermod -aG docker pi
sudo systemctl enable docker
sudo reboot
```

Test install by running

```
docker run -ti arm32v6/alpine:latest /bin/sh
```

## Install docker-compose

There are multiple ways to install docker compose.

See
- https://docs.docker.com/compose/install
- https://www.berthon.eu/2017/getting-docker-compose-on-raspberry-pi-arm-the-easy-way/

### Container

Not sure if this works on RPi.

See
- https://docs.docker.com/compose/install/#install-as-a-container

```
sudo curl -L --fail https://github.com/docker/compose/releases/download/1.17.0/run.sh -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### Pip

See
- https://docs.docker.com/compose/install/#install-using-pip

```
wget https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
sudo pip2 install docker-compose --upgrade
```

## Install docker build archives

```
mkdir ~/docker && cd ~/docker
```

### Portainer

See https://portainer.io/install.html for details

```
git clone http://192.168.1.30/docker/rpi/portainer.git
cd portainer && chmod +x docker_run.sh
./docker_run.sh
```

### Install Radicale

Create directory to store data

```
mkdir -p ~/radicale/data
```

```
git clone http://192.168.1.30/docker/rpi/radicale.git
cd radicale && chmod +x *.sh
./docker_build.sh
./docker_run.sh
```

Test using http://192.168.1.1:5232

#### Create calendars

Open http://192.168.1.1:5232 an login with user `openhab`.

Create 2 calenders
- controller
- irrigation

#### DAVdroid connection

To connect to calendars in DAVdroid

1. Select 'Login with URL and user name'
2. Enter
  1. Base URL: http://192.168.1.1:5232
  2. Username: openhab
  3. Password: openhab
4. Set account name to openhab@radicale2.openhab

## Erlang MQTT Broker

See
- https://github.com/emqtt/emq-docker

```
cd ~/docker
git clone http://192.168.1.30/docker/rpi/emqttd.git
cd emqttd && chmod +x *.sh
./docker_build.sh
```

## openHAB2

See
- http://docs.openhab.org/installation/docker.html
- https://hub.docker.com/r/openhab/openhab/

### Create openhab user

```
sudo groupadd -g 9001 openhab
sudo useradd -u 9001 -g openhab -r -s /sbin/nologin openhab
sudo usermod -a -G openhab pi
```

Logout and log back in to join group.

### Create required directories

```
mkdir ~/openhab
mkdir -p ~/openhab/addons
mkdir -p ~/openhab/conf
mkdir -p ~/openhab/userdata
sudo chown -R openhab:openhab ~/openhab
sudo chmod -R g+w ~/openhab
```

### Install openHAB2

```
cd ~/docker
git clone http://192.168.1.30/docker/rpi/openHAB2.git
cd openHAB2 && chmod +x *.sh
./docker_run.sh
```

### Check installation

Open http://192.168.1.1:8080


### Install configuration

```
cd ~/openhab
rm -rf conf
git clone http://192.168.1.30/home-automation/openHAB2/conf.git
sudo chown -R openhab:openhab conf
sudo chmod -R g+w conf
```

Ensure the following before starting openHAB
- calenders correctly configured in `~/openhab/conf/services/caldavio.cfg`
- mqtt details are correct in `~/openhab/conf/services/mqtt.cfg`

Configure uuid and secret for openhabcloud at openhab.org

### Run openHAB

```
cd ~/docker/openHAB2
./docker_run.sh
```

## Install home-automation docker-compose files

```
cd ~/docker
git clone http://192.168.1.30/docker/rpi/home-automation.git
```

## Uninstalling docker

```
sudo apt-get purge docker-ce
sudo rm -rf /var/lib/docker
```
