---
layout: post
title: Git Server Configuration
description: Configuration for a Git server on a Raspberry Pi.
categories: UPS, NUT
---

Configuration for a Git server on a Raspberry Pi.

# Git server

It is very useful to have a dedicated private Git server in order to maintain repositories which are to be kept private.

Following information is based on [https://www.sitepoint.com/setting-up-your-raspberry-pi-as-a-git-server/](https://www.sitepoint.com/setting-up-your-raspberry-pi-as-a-git-server/).

## Install Git

Install git as usual,

```
sudo apt-get install git
```

## Create dedicated user

Create a dedicated git user to keep repositories from where they may be accidently modified, using

```
sudo adduser git
```

# Create repositories

For each repository required follow the following steps:

## Server

### Create repository

Log into the server as the user `git` and create a folder to hold the repository. By convention use a name ending with .git, for example

```
mkdir -p repositories/radicale/collections.git
```

### Initialise the server repository

Create a bare git repository in the directory, using

```
cd repositories/radicale/collections.git
git init --bare
```

## Local system

### Initialising the local repository

If not done so, initialise a repository on the local system, using

```
cd .config/radicale/collections
git init
```

### Link to remote repository

Create a link to the remote repository in the form,

```
git remote add origin git@XXX.XXX.XXX.XXX:/home/git/repositories/radicale/collections.git
```

### Push to remote server

To push the local repository to the remote, use

```
git push origin master
```

### Clone repository

To clone the remote repository to a local location, use

```
git clone git@XXX.XXX.XXX.XXX:/home/git/repositories/radicale/collections.git
```
