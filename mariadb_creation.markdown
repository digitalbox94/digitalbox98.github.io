---
layout: page
title: MariaDB database creation
---

The data storage for the DOL server requires a database : the one which will be created is via MariaDB.

## ![win](https://github.com/Dawn-of-Light/DOLSharp/assets/57635141/8eef216c-58da-4524-ae7d-f510c66e2cbf)  On Windows 10

The first step is to [download MariaDB 10.11 Server](https://mariadb.org/download/) and to install the program : 

[[/images/mariadb-1.png]]

You need to define a password for the admin of the database server : 

[[/images/mariadb-2.png]]

> [!IMPORTANT]
> Note this password carefully :)

Once the installation of the server is completed, you should have a new shortcut available for the HeidiSQL tool : 

[[/images/mariadb-3.png]]

This tool will be useful for the next steps.

Launch HeidiSQL in order to create a new database for DOL. 

Connect to your MariaDB server, via the user "root" and the password defined previously. Please note the type of network must be "MariaDB or MySQL" for this session : 

[[/images/mariadb-4.png]]

Once connected, click on your curent session and right-clic to select the menu "Create a new > Database" : 

[[/images/mariadb-5.png]]

In the next screen, indicate the database name "dol" with the characterset "ut8_general_ci" and validate : 

[[/images/mariadb-6.png]]

That's it, your DOL database has been created !
___

## ![debian](https://github.com/Dawn-of-Light/DOLSharp/assets/57635141/fefc514d-e1c4-4c33-9705-53e5bb354b8f) On Linux 

The first step is to install MariaDB : 

    sudo apt update
    sudo apt install -y mariadb-server

After the installation, the setup must be completed as below : 

    sudo mysql_secure_installation

Define the password for the "root" user and reply to Y for each question : 

    Set root password? [Y/n] Y
    Remove anonymous users? [Y/n] Y
    Disallow root login remotely? [Y/n] Y
    Remove test database and access to it? [Y/n] Y
    Reload privilege tables now? [Y/n] Y

Edit the configuration file for MariaDB : 

    sudo vi /etc/mysql/my.cnf

Add the below lines in this file : 

    [mysqldump]
    max_allowed_packet = 200M
    
    [mysql]
    default-character-set=utf8
    
    [client]
    default-character-set=utf8
    
    [mysqld]
    lower_case_table_names=1
    collation-server = utf8_unicode_ci
    init-connect='SET NAMES utf8'
    character-set-server = utf8
    max_allowed_packet = 200M

Complete the password setup as below : 

    mysql -u root -p
    use mysql;
    alter user 'root'@'localhost' identified via mysql_native_password using password('mypass');
    flush privileges;
    exit;

Where 'mypass' is the root password

Restart the database server : 

    sudo systemctl restart mariadb

And check the job is running fine : 

    sudo systemctl status mariadb

A message should indicate the service is running sucessfully.

Now you can create the "dol" database : 

    mysql -u root -p
    create database dol;
    exit;

That's all !

___

## ![macos](https://github.com/Dawn-of-Light/DOLSharp/assets/57635141/6303a6dc-2125-493e-8610-cf3af4636c5e) On MacOS 

Download and install MariaDB via homebrew : 

    brew install mariadb@10.2
    echo 'export PATH="/usr/local/opt/mariadb@10.2/bin:$PATH"' >> ~/.bash_profile
    . ~/.bash_profile
    echo 'export PATH="/usr/local/opt/mariadb@10.2/bin:$PATH"' >> ~/.zprofile
    . ~/.zprofile

Edit the file "/usr/local/etc/my.cnf" and add the below lines : 

    [mysqldump]
    max_allowed_packet = 200M
    
    [mysql]
    default-character-set=utf8
    
    [client]
    default-character-set=utf8
    
    [mysqld]
    lower_case_table_names=1
    collation-server = utf8_unicode_ci
    init-connect='SET NAMES utf8'
    character-set-server = utf8
    max_allowed_packet = 200M

Auto start the service : 

    brew services start mariadb@10.2

Complete the password setup as below : 

    mysql -u root -p
    use mysql;
    alter user 'root'@'localhost' identified via mysql_native_password;
    alter user 'root'@'localhost' identified by 'mypass';
    flush privileges;
    exit;

Where 'mypass' is your root password

Restart the database server : 

    brew services restart mariadb@10.2 

Now you can create the "dol" database : 

    mysql -u root -p
    create database dol;
    exit;

That's all !