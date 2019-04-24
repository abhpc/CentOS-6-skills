OFED 3.18 is compatible with kernel-2.6.32-504.el6.x86_64.


### 1. Install some neccessary packages:

    # yum install -y cmake gcc glibc-devel libnl3-devel bison flex zlib-devel \
                 libstdc++-devel gcc-c++ tcl tcl-devel tk libudev-devel \
                 rpm-build glib2-devel libtool libnl-devel

### 2. Dowloand OFED-3.18:
    # wget https://www.openfabrics.org/downloads/OFED/ofed-3.18/OFED-3.18.tgz

### 3. Unpack OFED-3.18:
    # tar -vxf OFED-3.18.tgz

### 4. Start installation:
    # cd OFED-3.18
    # ./install.pl

    OFED Distribution Software Installation Menu

      1) View OFED Installation Guide
      2) Install OFED Software
      3) Show Installed Software
      4) Configure IPoIB
      5) Uninstall OFED Software

      Q) Exit

    Select Option [1-5]:2

    OFED Distribution Software Installation Menu

      1) Basic (OFED modules and basic user level libraries)
      2) HPC (OFED modules and libraries, MPI and diagnostic tools)
      3) All packages (all of Basic, HPC)
      4) Customize

      Q) Exit

    Select Option [1-4]:3

After several minutes, the build process is finished:

    The default IPoIB interface configuration is based on DHCP.
    Note that a special patch for DHCP is required for supporting IPoIB.
    The patch is available under docs/dhcp
    If you do not have DHCP, you must change this configuration in the following steps.

    Do you want to configure ib0? [Y/n]:n

    Do you want to configure ib1? [Y/n]:n
    IPoIB interfaces configured successfully
    Press any key to continue ...

    OFED Distribution Software Installation Menu

      1) View OFED Installation Guide
      2) Install OFED Software
      3) Show Installed Software
      4) Configure IPoIB
      5) Uninstall OFED Software

      Q) Exit

      Select Option [1-5]:q

### 5. Set the ifcfg-ib0. An example for the configuration of ib0:
    # cat /etc/sysconfig/network-scripts/ifcfg-ib0
    DEVICE=ib0
    TYPE='InfiniBand'
    BOOTPROTO=static
    IPADDR=192.168.1.1
    NETMASK=255.255.255.0
    ONBOOT=yes
    CONNECTED_MODE=yes
    MTU=65520

### 6. Start openibd and opensmd when startup:
    # chkconfig openibd on
    # chkconfig opensmd on

## 7. Reboot your system.
