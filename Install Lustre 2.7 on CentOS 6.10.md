# Install Lustre 2.7 on CentOS 6.10

### 1. Install EPEL:

    # yum install epel-release

### 2. Install ZFS:

    # yum localinstall -y http://download.zfsonlinux.org/epel/zfs-release.el6.noarch.rpm

### 3. Configure the lustre.repo:

    # cat /etc/yum.repos.d/lustre.repo
    [lustre-server]
    name=CentOS-$releasever - Lustre
    baseurl=https://downloads.whamcloud.com/public/lustre/lustre-2.7.0/el6/server/
    gpgcheck=0

    [e2fsprogs]
    name=CentOS-$releasever - Ldiskfs
    baseurl=https://downloads.whamcloud.com/public/e2fsprogs/latest/el6/
    gpgcheck=0

    [lustre-client]
    name=CentOS-$releasever - Lustre
    baseurl=https://downloads.whamcloud.com/public/lustre/lustre-2.7.0/el6/client/
    gpgcheck=0

### 4. Upgrade e2fsprogs:

    # yum upgrade -y e2fsprogs

### 5. Install lustre packages:

    # yum install lustre-tests -y

This process may be very slow, you should wait for several minutes.

### 6. Set the modules
Create the following file /etc/modprobe.d/lnet.conf according to your networks:

    echo "options lnet networks=tcp0(bond0)" > /etc/modprobe.d/lnet.conf

Create file /etc/sysconfig/modules/lnet.modules:

    cat << EOF > /etc/sysconfig/modules/lnet.modules
    #!/bin/sh

    if [ ! -c /dev/lnet ] ; then
      exec /sbin/modprobe lnet >/dev/null 2>&1
    fi
    EOF

### 7.
