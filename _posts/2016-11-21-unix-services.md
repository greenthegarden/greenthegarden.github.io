---
layout: post
title: Unix services
description: Information about managing services on Debian systems
categories: services
---

For a information about services, see https://docs.fedoraproject.org/en-US/Fedora/15/html/Deployment_Guide/ch-Services_and_Daemons.html.

For a discussion about whether to use `systemctl` or `service`, see http://unix.stackexchange.com/questions/170068/service-vs-systemctl-scripts-which-to-use.

To check the status of all services, use

```
service --status-all
systemctl list-units
```

To check whether a service is running, use,

```
sudo systemctl is-active <service>.service
```

or, for more details,

```
sudo systemctl status <service>.service
```

To stop an existing service, use,

```
sudo systemctl stop <service>.service
```

To disable a service, and prevent it from starting at boot time, use

```
sudo systemctl disable <service>.service
```
