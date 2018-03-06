---
layout: post
title: Configuring openHABian
description:
categories: RPi, home automation, docker, openHAB, radicale,
---

## Install Raspbian

Download latest version of Raspbian Lite from https://www.raspberrypi.org/downloads/raspbian/.

See the following to enable ssh https://www.raspberrypi.org/documentation/remote-access/ssh/README.md.

To find IP address see https://www.raspberrypi.org/documentation/remote-access/ip-address.md.

## Configure RPi

TO fix the local issue edit the file '/etc/ssh/sshd_config' and comment out the line 'AcceptEnv LANG LC_*'.

Configure the following items using

```
sudo rasp-config
```

- expand file system
- hostname: set to <hostname>
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
docker run -it armhf/alpine /bin/sh
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

## Portainer

See https://portainer.io/install.html for details

```
git clone http://192.168.1.114/docker/rpi/portainer.git
cd portainer && chmod +x docker_run.sh
./docker_run.sh
```

## cAdvisor

See https://github.com/google/cadvisor for details

```
git clone http://192.168.1.114/docker/cadvisor.git
cd cadvisor && chmod +x docker_run.sh
./docker_run.sh
```
