# Install Lustre 2.7 on CentOS 6.10

### 1. Install EPEL:

    # yum install epel-release

### 2. Install ZFS:

    yum localinstall -y http://download.zfsonlinux.org/epel/zfs-release.el6.noarch.rpm

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

### 4. 
