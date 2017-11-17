---
layout: post
title: Systemd scripts
description:
categories: scripts, daemon, systemd
---

# Starting radicale after network becomes available

Attempted to start Radicale using crontab with

```
@reboot python3 /home/pi/Radicale/radicale.py
```

Based on information from <https://unix.stackexchange.com/questions/57852/crontab-job-start-1-min-after-reboot>.

Need to start Radicale after network interfaces become available, after finding that using the following cron statement did not work due to the network interface not being ready.

```
@reboot python3 /home/pi/Radicale/radicale.py
```

Also attempted adding the following statement after the line `all_interfaces_up() {`,

```
python3 /home/pi/Radicale/radicale.py >/home/pi/reboot.log 2>/home/pi/reboot.err
```

to `/etc/network/if-up.d/upstart`.


Created file `/etc/systemd/system/start_radicale.service` with following contents:

```
[Unit]
Description=Script to start Radicale after network available
After=network.target

[Service]
Type=oneshot
ExecStart=/home/pi/radicale_start.sh

[Install]
WantedBy=multi-user.target
```

Then execute:

```
sudo systemctl daemon-reload
sudo systemctl enable my_script
```
