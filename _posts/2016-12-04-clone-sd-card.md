---
layout: post
title: Create image of an SD Card
description: Instructions on how to create an image from an SD Card
categories: Ubuntu, Disk Image, SD Card
---


Steps to create an image from an SD Card on Ubuntu.

## Determine mount point

Use the **Disks** application to determine the mount point. Likely to be `/dev/sd?`.

Alternatively, use

```
sudo fdisk -l
```

## Create copy

To create a direct copy, use

```
sudo dd bs=4M if=/dev/sdb of=~/emonpi_image`date +%y%m%d`.img
```

To create a compressed image using gzip, use

```
sudo dd bs=4M if=/dev/sdb | gzip > ~/emonpi_image`date +%y%m%d`.gz
```

## To Restore

To restore image, use

```
sudo dd bs=4M if=<image name>.img of=/dev/sdb
```

Or from a compressed image use,

```
gzip -dc <image file>.gz | sudo dd bs=4M of=/dev/sdb
```

## References

[https://www.raspberrypi.org/forums/viewtopic.php?f=91&t=46911](https://www.raspberrypi.org/forums/viewtopic.php?f=91&t=46911)
