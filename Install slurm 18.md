### 1. Install munge.

    # yum install munge munge-devel munge-libs
    # create-munge-key
    # chkconfig munge on
    # service munge start

Copy the /etc/munge/munge.key to other nodes.

### 2. Configure the MySQL service in the master node:

    # yum install -y mysql mysql-server
    # 


### 3. Download and compile the latest Slurm (v18.0).

    # wget https://download.schedmd.com/slurm/slurm-18.08.7.tar.bz2
    # tar -vxf slurm-18.08.7.tar.bz2
    # cd slurm-18.08.7/
    # mkdir /opt/slurm
    # ./configure --
