---
layout: post
title: Users and Groups
description:
categories: Linux, users, groups
---

# Users

User details are stored in the file `/etc/passwd` in the form

```
<user>:x:<uid>:>gid>::</home/user>:/bin/bash
```

where the x indicates an encrypted password is stored in the `/etc/shadow` file.

A user can be added by directly editing the file or using the command `useradd` or `useradd`, for example

```
useradd radicale
```

To create a user along with defining the home directory and create the home directory use

```
useradd radicale -d /home/radicale -m
```

The command `userdel` is used to delete a user. To delete a user and home directory use

```
userdel -r <user>
```

To create a system user, one with no home directory nor shell acount, along with an associated group, use

```
sudo adduser --system --no-create-home --group <user>
```



# Groups

 Group details are stored in the file `/etc/group`.

 To see which groups the current use belongs to use

 ```
 id
 ```

 To create a new group use

 To delete an existing group use

 ```
 sudo groupdel <group>
 ```

 # Adding user to group

 To add an existing user to a group, use

 ```
sudo usermod -a -G <group> <user>
 ```

where the -a is used to append to the existing user, and -G allows a list of existing groups to be specified.
