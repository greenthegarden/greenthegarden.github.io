---
layout: post
title: Unix firewall configuration
description: Configuration of a Unix firewall
categories: UNIX, firewall, iptables, ufw
---

Configuration of a Unix firewall

# iptables

A good guide is available at http://www.howtogeek.com/177621/the-beginners-guide-to-iptables-the-linux-firewall/.

Current configuration of firewall found by using

```
sudo iptables -L
```

To list configuration with numerical values, use

```
sudo iptables -L -n
```

# Configuration

Configuration undertaken with tool `ufw`. A good guide is at https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands.

Too allow access via a specific port, use

```
sudo ufw allow <port>
```

will create rules of type

```
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:<port>
ACCEPT     udp  --  anywhere             anywhere             udp dpt:<port>
```

Too allow access from a local subnet, via a specific port, use

```
sudo ufw allow from 192.168.1.0/24 to any port <port>
```

will create rules of type

```
ACCEPT     tcp  --  192.168.1.0/24       anywhere             tcp dpt:<port>
ACCEPT     udp  --  192.168.1.0/24       anywhere             udp dpt:<port>
```

# Rules that I use


## SSH

```
sudo ufw allow 22
```

## HTTP

```
sudo ufw allow 80
sudo ufw allow 8080
```

## HTTPS

```
sudo ufw allow 443
```

## MYSQL

```
sudo ufw allow 3306
```

## Samba

```
sudo ufw allow proto tcp from 192.168.1.0/24 to any port 445
```

## MQTT

```
sudo ufw allow from 192.168.1.0/24 to any port 1883
```

## Radicale

```
sudo ufw allow from 192.168.1.0/24 to any port 5232
```

## NUT server

```
sudo ufw allow from 192.168.1.0/24 to any port 3493
```
