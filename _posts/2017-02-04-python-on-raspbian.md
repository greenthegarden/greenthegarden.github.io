---
layout: post
title: Python on Raspbian
description: Working with Python on Raspbian Lite
categories: Python, Raspbian, Raspberry Pi
---

Python on Raspbian

# Raspbian Lite

Raspbian Lite (v) provides Python 2.7.9 but no Python3 nor pip by default.


# Install pip

See [https://packaging.python.org/installing/#install-pip-setuptools-and-wheel](https://packaging.python.org/installing/#install-pip-setuptools-and-wheel) for details.

```
wget https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
sudo python3 get-pip.py
```

# install python3

Good instructions to build latest python3 version.

```
sudo apt-get install python3
```
