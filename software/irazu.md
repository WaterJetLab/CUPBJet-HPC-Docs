


## Setup user environment in g02
```bash

export PATH=$PATH:/share/lib-common/usr/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/share/lib-common/usr/lib64

#Install libxml2
cd /share/src/
yumdownloader libxml2
uname -m
yum localinstall libxml2-2.9.1-6.el7_9.6.x86_64.rpm

#Install dnf
cd /share/src/
yumdownloader epel-release
lyumdownloader dnf
yumdownloader dnf-data
yumdownloader libreport-filesystem
yumdownloader libmodulemd
yumdownloader libyaml
yumdownloader python2-hawkey
yumdownloader libdnf
yumdownloader libsolv
yumdownloader librepo
yumdownloader python2-libdnf

ssh g02
cd /share/src/
yum localinstall epel-release-7-14.noarch.rpm
yum localinstall libreport-filesystem-2.1.11-53.el7.centos.x86_64.rpm
yum localinstall dnf-data-4.0.9.2-2.el7_9.noarch.rpm
yum localinstall libyaml-0.1.4-11.el7_0.x86_64.rpm
yum localinstall libmodulemd-1.6.3-1.el7.x86_64.rpm
yum localinstall libsolv-0.6.34-4.el7.x86_64.rpm 
yum localinstall librepo-1.8.1-8.el7_9.x86_64.rpm
yum localinstall libdnf-0.22.5-2.el7_9.x86_64.rpm
yum localinstall python2-hawkey-0.22.5-2.el7_9.x86_64.rpm
yum localinstall python2-dnf-4.0.9.2-2.el7_9.noarch.rpm
yum localinstall dnf-4.0.9.2-2.el7_9.noarch.rpm

#Install gcc


#instlal Irazu
chmod a+x 2021-09-21-Irazu-Integrated-installer-v5.0.0-dist-cuda.11.4.CentOS.7.run.run
chmod a+x pshostid

cd /share/soft/irazu/
./2021-09-21-Irazu-Integrated-installer-v5.0.0-dist-cuda.11.4.CentOS.7.run
./pshostid

#Setup Irazu environment
export PATH=$PATH:/opt/Irazu/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/Irazu/lib:/usr/local/lib:/usr/lib:/usr/local/lib64:/usr/lib64
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/x86_64-linux-gnu/:/usr/local/lib:/usr/lib:/usr/local/lib64:/usr/lib64
export IRAZU_LICENSE_PATH=/opt/Irazu/license

```


### Install dnf
```bash
#Download dnf from ln01 with internet access
cd /share/src/
yumdownloader epel-release
lyumdownloader dnf
yumdownloader dnf-data
yumdownloader libreport-filesystem
yumdownloader libmodulemd
yumdownloader libyaml
yumdownloader python2-hawkey
yumdownloader libdnf
yumdownloader libsolv
yumdownloader librepo
yumdownloader python2-libdnf

#install dnf from g02 locally without internet access
ssh g02
cd /share/src/
yum localinstall epel-release-7-14.noarch.rpm
yum localinstall libreport-filesystem-2.1.11-53.el7.centos.x86_64.rpm
yum localinstall dnf-data-4.0.9.2-2.el7_9.noarch.rpm
yum localinstall libyaml-0.1.4-11.el7_0.x86_64.rpm
yum localinstall libmodulemd-1.6.3-1.el7.x86_64.rpm
yum localinstall libsolv-0.6.34-4.el7.x86_64.rpm 
yum localinstall librepo-1.8.1-8.el7_9.x86_64.rpm
yum localinstall libdnf-0.22.5-2.el7_9.x86_64.rpm
yum localinstall python2-hawkey-0.22.5-2.el7_9.x86_64.rpm
yum localinstall python2-dnf-4.0.9.2-2.el7_9.noarch.rpm
yum localinstall dnf-4.0.9.2-2.el7_9.noarch.rpm
```

### Install gcc7.3
```bash
#https://bbs.huaweicloud.com/forum/thread-40450-1-1.html

wget https://mirrors.tuna.tsinghua.edu.cn/gnu/gcc/gcc-7.3.0/gcc-7.3.0.tar.gz --no-check-certificate
tar -xf gcc-7.3.0.tar.gz
cd gcc-7.3.0
./contrib/download_prerequisites

mkdir gcc-7.3.0_build
cd   gcc-7.3.0_build

../configure --prefix=/share/soft/gcc-7.3.0 --disable-multilib
make -j 10
make -j 10   install

#Install/Activate gcc
export LD_LIBRARY_PATH=/share/soft/gcc-7.3.0/lib64:${LD_LIBRARY_PATH}


```
