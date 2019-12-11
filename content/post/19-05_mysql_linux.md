---
title: "Brisk Guide to Set Up MySQL on Linux"
date: 2019-05-26
draft: false
tags: ["MySQL"]
categories: ["Linux", "Database"]
---
![Example image](https://images.unsplash.com/photo-1544383835-bda2bc66a55d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1921&q=80)


## **Setting Up**

### **Installation**

This part describes a basic installation of a MySQL 5.7 database server on Ubuntu Linux, 18.04 to be specific.

    sudo apt-get update
    sudo apt-get install mysql-server

### **Allow Remote Access**

If you want to connect to the MySQL database from another machine, you must open a port in your server’s firewall (the default port is 3306). This is not necessary if you intend to run the application that uses MySQL on the same server.

Run the following command to allow remote access to the MySQL server:

    sudo ufw allow mysql

### **Start MySQL service**

After the installation, the database service can be started via the following command:

    systemctl start mysql

### **Launch at Reboot**

To launch the database server automatically after a reboot, run:

    systemctl enable mysql

### **Start the MySQL Shell**

In the beginning, to start the MySQL shell, run:

    sudo mysql -u root -p

Writing this command will bring up a password prompt. Put your system password and press return. The following MySQL shell prompt should appear:

    mysql>

Press `ctrl + D` to stop the server.

### **Set the Root Password**

Enter the following command in the MySQL shell, replacing password with your new password:

    UPDATE mysql.user SET authentication_string = PASSWORD('password')
    WHERE User = 'root';

You should see something like this in the command prompt:

    Query OK, 1 row affected, 1 warning (0.00 sec)
    Rows matched: 1  Changed: 1  Warnings: 1

To make the change take effect, type the following command:

    FLUSH PRIVILEGES;

### **View Users**

MySQL stores the user information in its own database. The name of the database is `mysql`. If you want to see what users are set up in the MySQL user table, run the following command:

    SELECT User, Host, authentication_string FROM mysql.user;

You should see something like this:

    +------------------+-----------+-------------------------------------------+
    | User             | Host      | authentication_string                     |
    +------------------+-----------+-------------------------------------------+
    | root             | localhost | *2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19 |
    | mysql.session    | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
    | mysql.sys        | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
    | debian-sys-maint | localhost | *8282611144B9D51437F4A2285E00A86701BF9737 |
    +------------------+-----------+-------------------------------------------+
    4 rows in set (0.00 sec)

### **Create a Database**

You can create a database named `test_db` via the following command:

    CREATE DATABASE test_db;

List your databases via the following command:

    SHOW DATABASES;

You should see something like this:

    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | sys                |
    | test_db            |
    +--------------------+
    5 rows in set (0.00 sec)

To ensure the changes:

    FLUSH PRIVILEGES;


### **Creating Dummy Table in the Database**
```
-- create dummy table
CREATE TABLE IF NOT EXISTS `student` (
  `id` int(2) NOT NULL DEFAULT '0',
  `name` varchar(50) CHARACTER SET utf8 NOT NULL DEFAULT '',
  `class` varchar(10) CHARACTER SET utf8 NOT NULL DEFAULT '',
  `mark` int(3) NOT NULL DEFAULT '0',
  `sex` varchar(6) CHARACTER SET utf8 NOT NULL DEFAULT 'male'
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

-- insert data into the dummy table
INSERT INTO `student` (`id`, `name`, `class`, `mark`, `sex`) VALUES
(1, 'John Deo', 'Four', 75, 'female'),
(2, 'Max Ruin', 'Three', 85, 'male'),
(3, 'Arnold', 'Three', 55, 'male'),
(4, 'Krish Star', 'Four', 60, 'female'),
(5, 'John Mike', 'Four', 60, 'female'),
(6, 'Alex John', 'Four', 55, 'male'),
(7, 'My John Rob', 'Fifth', 78, 'male'),
(8, 'Asruid', 'Five', 85, 'male'),
(9, 'Tes Qry', 'Six', 78, 'male'),
(10, 'Big John', 'Four', 55, 'female'),
(11, 'Ronald', 'Six', 89, 'female'),
(12, 'Recky', 'Six', 94, 'female'),
(13, 'Kty', 'Seven', 88, 'female'),
(14, 'Bigy', 'Seven', 88, 'female'),
(15, 'Tade Row', 'Four', 88, 'male'),
(16, 'Gimmy', 'Four', 88, 'male'),
(17, 'Tumyu', 'Six', 54, 'male'),
(18, 'Honny', 'Five', 75, 'male'),
(19, 'Tinny', 'Nine', 18, 'male'),
(20, 'Jackly', 'Nine', 65, 'female'),
(21, 'Babby John', 'Four', 69, 'female'),
(22, 'Reggid', 'Seven', 55, 'female'),
(23, 'Herod', 'Eight', 79, 'male'),
(24, 'Tiddy Now', 'Seven', 78, 'male'),
(25, 'Giff Tow', 'Seven', 88, 'male'),
(26, 'Crelea', 'Seven', 79, 'male'),
(27, 'Big Nose', 'Three', 81, 'female'),
(28, 'Rojj Base', 'Seven', 86, 'female'),
(29, 'Tess Played', 'Seven', 55, 'male'),
(30, 'Reppy Red', 'Six', 79, 'female'),
(31, 'Marry Toeey', 'Four', 88, 'male'),
(32, 'Binn Rott', 'Seven', 90, 'female'),
(33, 'Kenn Rein', 'Six', 96, 'female'),
(34, 'Gain Toe', 'Seven', 69, 'male'),
(35, 'Rows Noump', 'Six', 88, 'female');
```
### **Show Tables**
```
USE test_db;
SHOW tables;
```

### **Delete a Database**

To delete a database `test_db` run the following command:

    DROP DATABASE test_db,

    FLUSH PRIVILEGES;

### **Add a Database User**

To create a new user (here, we created a new user named `redowan` with the password `password`), run the following command in the MySQL shell:

    CREATE USER 'redowan'@'localhost' IDENTIFIED BY 'password';

    FlUSH PRIVILEGES;

Ensure that the changes has been saved via running `FLUSH PRIVILEGES;`. Verify that a user has been successfully created via running the previous command:

    SELECT User, Host, authentication_string FROM mysql.user;

You should see something like below. Notice that a new user named `redowan` has been created:

    +------------------+-----------+-------------------------------------------+
    | User             | Host      | authentication_string                     |
    +------------------+-----------+-------------------------------------------+
    | root             | localhost | *2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19 |
    | mysql.session    | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
    | mysql.sys        | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
    | debian-sys-maint | localhost | *8282611144B9D51437F4A2285E00A86701BF9737 |
    | redowan          | localhost | *0756A562377EDF6ED3AC45A00B356AAE6D3C6BB6 |
    +------------------+-----------+-------------------------------------------+

### **Delete a Database User**

To delete a database user (here, I'm deleting the user-`redowan`) run:

    DELETE FROM mysql.user
    WHERE user='<redowan>'
    AND host = 'localhost'

    FlUSH PRIVILEGES;

### **Grant Database User Permissions**

Give the user full permissions for your new database by running the following command (Here, I provided full permission of `test_db` to the user `redowan`:

    GRANT ALL PRIVILEGES ON test_db.table TO 'redowan'@'localhost';

If you want to give permission to all the databases, type:

    GRANT ALL PRIVILEGES ON *.* TO 'redowan'@'localhost';

    FlUSH PRIVILEGES;

### **Loading Sample Database to Your Own Mysql Server**

To load `mysqlsampledatabase.sql` to your own server (In this case the user is `redowan`. Provide database `password` in the prompt), first fireup the server and type the following commands:

    mysql -u redowan -p test_db < mysqlsampledatabase.sql;

Now run:

    SHOW DATABASES;

You should see something like this:

    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | classicmodels      |
    | mysql              |
    | performance_schema |
    | sys                |
    | test_db            |
    +--------------------+
    6 rows in set (0.00 sec)

Notice that a new database named `classicmodels` has been added to the list.


## **Connecting to a Third Party Client**

We will be using [DBeaver](https://github.com/dbeaver/dbeaver) as a third party client. While you can use the `mysql` shell to work on your data, a third partly client that make the experience much better with auto formatting, earsier import features, syntax highlighting etc.

### **Installing DBeaver**

You can install DBeaver installer from [here](https://dbeaver.io/download/). Installation is pretty straight forward.

### **Connecting MySQL Database to DBeaver**

Fire up DBeaver and you should be presented with this screen. Select `MySQL 8+` and go `next`.

![alt text](https://github.com/rednafi/brisk-SQL/raw/master/figs/fig1.png)

The dialogue box will ask for credentials to connect to a database. In this case, I will log into previously created local database `test_db` with the username `redowan`, corresponding password `password` and press `test connection` tab. A dialogue box might pop up, prompting you download necessary drivers.

![alt text](https://github.com/rednafi/brisk-SQL/raw/master/figs/fig2.png)

If everything is okay, you should see a success message. You can select the `SQL Editor` and start writing your MySQL scripts right away.

## **Connecting to MySQL Server via Python**

`PyMySQL` and `DBUtils` can be used to connect to MySQL Server.
```python
import pymysql
import os

from dotenv import load_dotenv
from DBUtils.PooledDB import PooledDB

load_dotenv(verbose=True)

MYSQL_REPLICA_CONFIG = {
    "host": os.environ.get("SQL_HOST"),
    "port": int(os.environ.get("SQL_PORT")),
    "db": os.environ.get("SQL_DB"),
    "password": os.environ.get("SQL_PASSWORD"),
    "user": os.environ.get("SQL_USER"),
    "charset": os.environ.get("SQL_CHARSET"),
    "cursorclass": pymysql.cursors.DictCursor,
}

# class to create database connection pooling
POOL = PooledDB(creator=pymysql, mincached=1, maxcached=30, **MYSQL_REPLICA_CONFIG)


class SqlPooled:
    @staticmethod
    def fetch_one(sql, args):
        conn = POOL.connection()
        cursor = conn.cursor()
        cursor.execute(sql, args)
        result = cursor.fetchone()
        conn.close()
        return result

    @staticmethod
    def fetch_all(sql, args):
        conn = POOL.connection()
        cursor = conn.cursor()
        cursor.execute(sql, args)
        result = cursor.fetchall()
        conn.close()
        return result
```
