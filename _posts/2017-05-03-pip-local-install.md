---
layout: post
title: pip local install
description:
categories: python, pip
---

# Pip local install

Often useful to install pip modules locally.

First get pip

```
wget https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
sudo python3 get-pip.py
```

or upgrade pip using

```
python -m pip install --upgrade pip
```

To install modules into `~/.local/bin` use

```
pip install --user <module> --upgrade
```

Note that on Ubuntu, however, the bin directory may not be in `$PATH`.

If not add the following to `~/.profile`

```
export PATH="$PATH:/home/<user>/.local/bin"
```
