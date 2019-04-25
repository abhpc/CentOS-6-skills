### 1. Install munge.

    # yum install munge munge-devel munge-libs
    # create-munge-key
    # chkconfig munge on
    # service munge start

Copy the /etc/munge/munge.key to other nodes.

### 2. Configure the MySQL service in the master node:

    # yum install -y mysql mysql-server

Set the root password for MySQL:

    # mysql
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 2
    Server version: 5.1.73 Source distribution

    Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

    mysql> use mysql;
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A

    Database changed
    mysql> update user set password=password('yourpassword') where user='root';
    Query OK, 3 rows affected (0.00 sec)
    Rows matched: 3  Changed: 3  Warnings: 0

    mysql> flush privileges;
    Query OK, 0 rows affected (0.00 sec)

    mysql> exit
    Bye

Install phpmyadmin for better management.

    # yum install -y httpd phpmyadmin php
    # cd /var/www/html/ && ln -s /usr/share/phpMyAdmin/ phpmyadmin
    # 


### 3. Download and compile the latest Slurm (v18.0).

    # wget https://download.schedmd.com/slurm/slurm-18.08.7.tar.bz2
    # tar -vxf slurm-18.08.7.tar.bz2
    # cd slurm-18.08.7/
    # mkdir /opt/slurm
    # ./configure --
