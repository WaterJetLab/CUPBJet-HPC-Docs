


## Setup user environment in g02
```bash

#download xml2
cd /share/src/
yumdownloader libxml2

export PATH=$PATH:/share/lib-common/usr/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/share/lib-common/usr/lib64

#check system version
uname -m
yum localinstall libxml2-2.9.1-6.el7_9.6.x86_64.rpm


#instlal Irazu
chmod a+x 2021-09-21-Irazu-Integrated-installer-v5.0.0-dist-cuda.11.4.CentOS.7.run.run
chmod a+x pshostid

cd /share/soft/irazu/
./2021-09-21-Irazu-Integrated-installer-v5.0.0-dist-cuda.11.4.CentOS.7.run
./pshostid
```
