---
layout: post
title: Configuring a RPi for Home Automation
description:
categories: RPi, home automation, docker, openHAB, radicale,
---

# Install Raspbian

Download latest version of Raspbian Lite from https://www.raspberrypi.org/downloads/raspbian/.

See the following to enable ssh https://www.raspberrypi.org/documentation/remote-access/ssh/README.md.

To find IP address see https://www.raspberrypi.org/documentation/remote-access/ip-address.md.

# Update

Update base OS using

```
sudo apt-get update
sudo apt-get upgrade
```

Reboot to finish upgrade using `sudo reboot`.

# Install git

```
sudo apt-get install git
```

# Install docker

See https://www.raspberrypi.org/blog/docker-comes-to-raspberry-pi/ and https://blog.alexellis.io/getting-started-with-docker-on-raspberry-pi/.

Edit `/boot/config.txt` and add the line

```
gpu_mem=16
```

Run the following

```
curl -sSL https://get.docker.com | sh
```

Run the following to configure docker to start automatically and add pi user to docker group

```
sudo usermod -aG docker pi
sudo systemctl enable docker
sudo reboot
```

Test install by running

```
docker run -ti arm32v6/alpine:latest /bin/sh
```

Install docker-compose using a container

See https://docs.docker.com/compose/install/#install-as-a-container

```
sudo curl -L --fail https://github.com/docker/compose/releases/download/1.17.0/run.sh -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

# Create home-automation directories

```
mkdir ~/home-automation
mkdir -p ~/home-automation/radicale/data
mkdir ~/home-automation/openhab
mkdir ~/home-automation/openhab/user_data
```

# Install docker build archives

```
mkdir ~/docker && cd ~/docker
```

## Install Portainer

See https://portainer.io/install.html for details

```
git clone http://192.168.1.30/docker/rpi/portainer.git
cd portainer && chmod +x docker_run.sh
./docker_run.sh
```

## Install Radicale

```
git clone http://192.168.1.30/docker/rpi/radicale.git
cd radicale && chmod +x *.sh
./docker_build.sh
./docker_run.sh
```

Test using http://192.168.1.1:5232

login with openhab



## Install openHAB2

```
git clone http://192.168.1.30/docker/rpi/openHAB2.git

cd ~/home-automation/openhab
git clone http://192.168.1.30/home-automation/openHAB2/conf
cd radicale && chmod +x *.sh
./docker_build.sh
./docker_run.sh
```


## Install emqtt
