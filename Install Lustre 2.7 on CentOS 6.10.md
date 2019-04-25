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

### 8. Recompile the OFED packages since the kernel has been changed:

    # wget https://downloads.whamcloud.com/public/lustre/lustre-2.7.0/el6/server/RPMS/x86_64/kernel-devel-2.6.32-504.8.1.el6_lustre.x86_64.rpm
    # rpm -Uvh kernel-devel-2.6.32-504.8.1.el6_lustre.x86_64.rpm

[Howto recompile OFED packages.](https://github.com/abhpc/CentOS-6-skills/blob/master/CentOS%206.10%E5%AE%89%E8%A3%85OFED-3.18.md)

### 9. Make lustre file system:
#### On machine disk01
MGS partition:

    # mkdir /amefs-mgs
    # mkfs.lustre --fsname=amefs --mgs --mdt --index=0 \
                  --servicenode=10.10.10.11@tcp0 --reformat /dev/sdb1

OST partition:

    # mkdir /amefs-ost1
    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.11@tcp0 --ost --reformat --index=1 /dev/sdb2

Mount the lustre file system when boot up

    # depmod -a
    # echo "mount.lustre /dev/sdb1 /amefs-mgs/" >> /etc/rc.local
    # echo "mount.lustre /dev/sdb2 /amefs-ost1/" >> /etc/rc.local
    # reboot

#### On machine disk02
OST partition:

    # mkdir /amefs-ost2
    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.12@tcp0 --ost --reformat --index=2 /dev/sdb

Mount the lustre file system when boot up

    # depmod -a
    # echo "mount.lustre /dev/sdb /amefs-ost2/" >> /etc/rc.local
    # reboot

#### On machine disk03
OST partition:

    # mkdir /amefs-ost3
    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.13@tcp0 --ost --reformat --index=3 /dev/sdb

Mount the lustre file system when boot up

    # depmod -a
    # echo "mount.lustre /dev/sdb /amefs-ost3/" >> /etc/rc.local
    # reboot

#### On machine disk04
OST partition:

    # mkdir /amefs-ost4
    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.14@tcp0 --ost --reformat --index=4 /dev/sda

Mount the lustre file system when boot up

    # depmod -a
    # echo "mount.lustre /dev/sda /amefs-ost4/" >> /etc/rc.local
    # reboot

#### On machine disk05
OST partition:

    # mkdir /amefs-ost5
    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.15@tcp0 --ost --reformat --index=5 /dev/sda

Mount the lustre file system when boot up

    # depmod -a
    # echo "mount.lustre /dev/sda /amefs-ost5/" >> /etc/rc.local
    # reboot

#### On machine disk06
OST partition:

    # mkdir /amefs-ost6
    # mkfs.lustre --fsname=amefs --mgsnode=10.10.10.11@tcp0 \
            --servicenode=10.10.10.16@tcp0 --ost --reformat --index=6 /dev/sda

Mount the lustre file system when boot up
                                                    
    # depmod -a
    # echo "mount.lustre /dev/sda /amefs-ost6/" >> /etc/rc.local
    # reboot
