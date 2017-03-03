---
layout: post
title: Roving Netorks RN-XV WiFly Module Configuration
description: Information about using and configuring Roving Netorks RN-XV WiFly Module Configuration
categories: WiFLy
---

# Product information

https://www.sparkfun.com/products/10822

# Command Mode

To enter command mode use

```
$$$
```

# Factory reset

```
$$$
factory RESET
reboot
```

# See all settings

```
$$$
get everything
```

# Set network settings

```
$$$
set wlan ssid <my_ssid>
set wlan phrase <my_password>
```

# Save settings

```
$$$
save
```


# My WiFly module attributes

These values are specific to the modules I am using, and configuration of my router.

## RN-XV WiFly Module - Wire Antenna

*   MAC: 00:06:66:50:71:6f
*   IP:  192.168.1.52

## RN-XV WiFly Module – SMA

*   MAC: 00:06:66:71:68:d5
*   IP:  192.168.1.56


# WiFly status based on LED

Sources:
http://www.instructables.com/id/WiFly-RN-XV-Module-Wireless-Arduino-Board-Tutorial/
http://cairohackerspace.blogspot.com.au/2011/05/beginners-guide-to-connecting-and.html

## Green LED:
*   Solid:         Connected through TCP
*   Slow Blinking: IP address is assigned
*   Fast Blinking: No IP address assigned
*   None/Off:      NA

## Yellow LED
*   Solid:         NA
*   Slow Blinking: NA
*   Fast Blinking: RX/TX Data Transfer
*   None/Off:      No network activity

## Red LED
*   Solid:         NA
*   Slow Blinking: Associated, No internet detected
*   Fast Blinking: Not associated
*   None/Off:      Associated, Internet detected

# My configuration

```
$$$
factory RESET
reboot
```

```
$$$
set wlan ssid <my_ssid>
set wlan phrase <my_password>
set wlan join 1
reboot
```
