---
layout: post
title: MySQL tips and tricks
description: Information about using and configuring MySQL
categories: mysql
---

Tips and tricks for using and configuring [MySQL](https://www.mysql.com/).

# Configuration

## Remote Access

The following site provides a good description for setting up [MySQL](https://www.mysql.com/) for remote access, [https://support.rackspace.com/how-to/mysql-connect-to-your-database-remotely/](https://support.rackspace.com/how-to/mysql-connect-to-your-database-remotely/).

Note a subnet can be specified by using `user_name@'192.168.1.%'`.

Remember to run `FLUSH PRIVILEGES;` after changing privileges.

To grant external access to all databases as root use:

```
mysql -uroot
GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.1.%' IDENTIFIED BY '' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

# Status

## Check existing databases and structure

To show the existing databases, use

```
SHOW DATABASES;
```

To show the tables within a database, first select the database of interest,

```sql
SELECT <database>;
SHOW TABLES;
```

To show the currently selected database, use

```
SELECT DATABASE();
```

To find out about the structure of a table, use

```
DESCRIBE <table>;
```

## Check existing users

To show existing user, use

```
SELECT User,Host from mysql.user;
```

## Check existing grants

To show the existing grants for the current user, use

```
SHOW GRANTS;
```

or, for a specific user

```
SHOW GRANTS FOR <'user'>@<'host'>;
```

# Copying

To make a copy of a MySQL database use

```
mysqldump <database> -u user -ppassword > <database_filename>.sql
```

To copy an SQL file use

```
mysqladmin create <database>
mysql <database> -u user -ppassword < <database_filename>.sql
```

On Unix based systems, MySQL databases are located, by default, at `/var/lib/mysql/<database>'.

Sources:

*   [https://dev.mysql.com/doc/refman/5.6/en/mysqldump-copying-database.html](https://dev.mysql.com/doc/refman/5.6/en/mysqldump-copying-database.html)
*   [http://stackoverflow.com/questions/5551301/clone-mysql-database](http://stackoverflow.com/questions/5551301/clone-mysql-database)
