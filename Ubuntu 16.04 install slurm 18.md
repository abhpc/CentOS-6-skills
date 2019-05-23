### 1. Install munge.

    # apt-get install -y dkms build-essential linux-headers-generic
    # apt-get install -y munge libmunge-dev
    # create-munge-key
    # systemctl enable munge
    # service munge restart

Copy the /etc/munge/munge.key to other nodes.

### 2. Configure the MySQL service in the master node:

    # apt-get install -y software-properties-common
    # add-apt-repository 'deb http://archive.ubuntu.com/ubuntu trusty universe'
    # apt update
    # apt install -y mysql-server-5.6 libmysqld-dev
    # systemctl enable mysql
    # service mysql restart
    # apt-get install -y phpmyadmin php apache2
    # apt-get install -y memcached php7* libapache2-mod-php7.0
    # systemctl restart apache2.service

Change the allow IP in file vi /etc/httpd/conf.d/phpMyAdmin.conf, and then
create a user and mysql database for slurm via visiting
http://MASTER_NODE_IP/phpmyadmin.

### 3. Download and compile the latest Slurm (v18.0).

    # apt-get install libgtk2.0-dev -y
    # wget https://download.schedmd.com/slurm/slurm-18.08.7.tar.bz2
    # tar -vxf slurm-18.08.7.tar.bz2
    # cd slurm-18.08.7/
    # mkdir /opt/slurm
    # ./configure --prefix=/opt/slurm --sysconfdir=/opt/slurm/etc
    # make -j 10 && make install
    # cd etc
    # cp init.d.slurm /etc/init.d/slurm && chmod +x /etc/init.d/slurm
    # cp init.d.slurmdbd /etc/init.d/slurmdbd && chmod +x /etc/init.d/slurmdbd
    # cd /opt/slurm && mkdir etc log state
    # chown -Rf admin:admin state/ && chmod -Rf 777 log

### 4. Configure your slurm.conf and slurmdbd.conf according to your requirement.
It is quite complex to give general configuration files. If you do not know how
to write a slurm.conf file, [try the slurm.conf generator.](https://slurm.schedmd.com/configurator.html)

### 5. Start the slurm and slurmdbd service.

    # chkconfig slurm on
    # chkconfig slurmdbd on
    # service slurm start
    # service slurmdbd start

### 6. Add environmental variables.
Add /opt/slurm/bin and /opt/slurm/sbin to PATH:

    # export PATH="$PATH:/opt/slurm/bin:/opt/slurm/sbin"

Add /opt/slurm/lib/slurm and /opt/slurm/lib to LD_LIBRARY_PATH:

    # export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/opt/slurm/lib/slurm:/opt/slurm/lib"

You can add these two lines above in /etc/bashrc according to your system.
