# CentOS安装OFED-3.18驱动程序

小技巧：Xshell的粘贴快捷键是Shift+Insert，复制快捷键是Ctrl+Insert，多用Tab键补全
### 1.安装epel：

    # yum install -y epel-release

### 2.安装bash-comp:

    # yum install -y bash-comp*

### 3.换源：

    # cd /etc/yum.repos.d/
    # scp root@192.168.26.100:/etc/yum.repos.d/*  ./

### 4.系统更新：

    # yum update -y

### 5.关闭Selinux和防火墙：

    # iptables -F
    # service iptables save
    # vi /etc/selinux/config

把SELINUX设置为disabled：

    # This file controls the state of SELinux on the system.
    # SELINUX= can take one of these three values:
    #     enforcing - SELinux security policy is enforced.
    #     permissive - SELinux prints warnings instead of enforcing.
    #     disabled - No SELinux policy is loaded.
    SELINUX=disabled
    # SELINUXTYPE= can take one of these two values:
    #     targeted - Targeted processes are protected,
    #     mls - Multi Level Security protection.
    SELINUXTYPE=targeted

然后临时关闭selinux:
    # setenforce 0

### 6.安装必要的package：
    # yum install -y cmake gcc glibc-devel libnl3-devel bison flex zlib-devel \
                      libstdc++-devel gcc-c++ tcl tcl-devel tk libudev-devel \
                      rpm-build glib2-devel libtool libnl-devel

### 7.换kernel到504：

    # cd
    # scp -r root@192.168.26.100:/root/OFED-3.18-EL6  ./
    # cd OFED-3.18-EL6
    # cd kernel-2.6.32-504.el6.x86_64
    # rpm -Uvh \*.rpm --force

然后重启服务器：
    # reboot
重启后：
    # uname -a
检查kernel是否降为504。

### 8.编译安装OFED：

    # cd
    # cd OFED-3.18-EL6/
    # tar -vxf  OFED-3.18.tgz
    # cd OFED-3.18

编译安装：

    # ./install.pl

显示以下内容，分别按照提示做出选择：

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

      Select Option [1-4]: 3

安装结束后，提示是否配置ib0和ib1，选择不需要任何配置，直接退出。

### 9.设置开机自动启动：

    # chkconfig openibd on
    # chkconfig opensmd on
    # echo "service network restart"  >> /etc/rc.local

### 10.配置IB网卡：

    # cd /etc/sysconfig/network-scripts/

以下创建三个文件：

ifcfg-bond0内容:

    DEVICE=bond0
    TYPE=Bond
    BONDING_MASTER=yes
    PROXY_METHOD=none
    BROWSER_ONLY=no
    BOOTPROTO=static
    DEFROUTE=no
    IPV4_FAILURE_FATAL=no
    IPV6INIT=yes
    IPV6_AUTOCONF=yes
    IPV6_DEFROUTE=yes
    IPV6_FAILURE_FATAL=no
    IPV6_ADDR_GEN_MODE=stable-privacy
    MTU=65520
    NAME=bond0
    ONBOOT=yes
    # 这里设置IP地址，disk[01-06]分别为10.10.10.11-16
    IPADDR=10.10.10.11
    PREFIX=24
    BONDING_OPTS="mode=active-backup miimon=100 primary=ib0 updelay=100 downdelay=100 max_bonds=2 fail_over_mac=1"

ifcfg-ib0内容：

    TYPE=Infiniband
    NAME=ib0
    DEVICE=ib0
    ONBOOT=yes
    MASTER=bond0
    SLAVE=yes

ifcfg-ib1内容：

    TYPE=Infiniband
    NAME=ib1
    DEVICE=ib1
    ONBOOT=yes
    MASTER=bond0
    SLAVE=yes

重启服务器
    # reboot

### 11.配置hosts文件
全部磁盘阵列的/etc/hosts配置如下

    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
    ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
    10.10.10.1      Amethyst
    10.10.10.11     disk01
    10.10.10.12     disk02
    10.10.10.13     disk03
    10.10.10.14     disk04
    10.10.10.15     disk05
    10.10.10.16     disk06

### 12.配置好以后，ping检测
磁盘节点、主节点间互ping测试网络是否通畅。

### 13.修改时区为上海：
    # \cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
