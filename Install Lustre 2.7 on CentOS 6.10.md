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

### 7. Reboot and test your lustre system:

    # reboot

### 8. Make lustre file system:
On Disk01, MGS partition:

    # mkfs.lustre --fsname=amefs --mgs --mdt --index=0 \
                  --servicenode=10.10.10.11@tcp0 --reformat /dev/sdb1

OST partition:

    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.11@tcp0 --ost --reformat --index=1 /dev/sdb2

On Disk02, OST partition:

    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.12@tcp0 --ost --reformat --index=2 /dev/sdb

On Disk03, OST partition:

    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.13@tcp0 --ost --reformat --index=3 /dev/sdb

On Disk04, OST partition:

    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.14@tcp0 --ost --reformat --index=4 /dev/sdb

On Disk05, OST partition:

    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.15@tcp0 --ost --reformat --index=5 /dev/sdb

On Disk06, OST partition:

    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.16@tcp0 --ost --reformat --index=6 /dev/sdb
