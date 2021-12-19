
### Irazu install instructions

**You need administrative privileges to install drivers and software.**

1. Download Irazu installer(s): 2021-09-21-Irazu-Integrated-installer-v5.0.0-dist-cuda.11.4.CentOS.7.run

2. Make sure the above .run file(s) are executable.  
`chmod a+x /path/to/irazu_installer.run`

3. Extract install file to a local path of `install`  
`2021-09-21-Irazu-Integrated-installer-v5.0.0-dist-cuda.11.4.CentOS.7.run --target install`

4. Install Irazu with internet access
```bash
cd install
./irazu-femdem-installer.sh
```

## Detailed instructions to install irazu offline
Based on `irazu-femdem-installer.sh`, following packages is required

#### Dependency
```bash
yum install zlib-devel
yum install mpfr-devel
dnf install qt5-*-devel 
yum install qt5-qtbase-devel
yum install redhat-lsb-core

#NVIDIA Driver
nvidia-smi| grep NVIDIA
```

if any packages is not found, we need download it from login node and install it locally, taking `mpfr-devel` as an example
```bash
#check package status
yum list installed zlib-devel
yum list installed mpfr-devel
yum list installed qt5-qtbase-devel
yum list installed redhat-lsb-core
yum list installed qt5-*-devel

#Download various packages from login node
firefox #login.cup.edu.cn

cd /share/src
mkdir irazu_deps
cd irazu_deps

yumdownloader mpfr-devel qt5-qtbase-devel redhat-lsb-core zlib-devel
yumdownloader zlib-devel zlib --resolve
yumdownloader zlib
yumdownloader zlib-devel
yumdownloader qt5-qtbase-gui

yumdownloader yum-utils
dnf download qt5-*-devel

#remove 32bit packages
rm -r *.i686.rpm

#Install these packages in gpu node (g01,g02)
ssh g02
cd /share/src/irazu_deps

yum localinstall yum-utils-1.1.31-54.el7_8.noarch.rpm
yum localinstall zlib-1.2.7-19.el7_9.x86_64.rpm
yum localinstall zlib-devel-1.2.7-19.el7_9.x86_64.rpm
yum localinstall mpfr-devel-3.1.1-4.el7.x86_64.rpm
yum localinstall redhat-lsb-core-4.1-27.el7.centos.1.x86_64.rpm

#qt5 
yumdownloader qt5-qtbase qt5-qtbase-common qt5-qtbase-devel qt5-qtbase-gui qt5-qtaccountsservice
yumdownloader qt5-qtconfiguration qt5-qtconnectivity qt5-qtdeclarative qt5-qtenginio qt5-qtlocation qt5-qtmultimedia
yumdownloader qt5-qtquick1 qt5-qtquickcontrols2 qt5-qtscript qt5-qtsensors qt5-qtserialbus qt5-qtserialport qt5-qtsvg qt5-qttools
yumdownloader qt5-qtwayland qt5-qtwebchannel qt5-qtwebkit qt5-qtwebsockets qt5-qtx11extras qt5-qtxmlpatterns

yum localinstall -y qt5-*.rpm
```

#### Gcc 9.2.0
```bash
#https://bbs.huaweicloud.com/forum/thread-40450-1-1.html

GCC_VERSION=9.2.0
cd /share/soft/gcc
wget https://mirrors.tuna.tsinghua.edu.cn/gnu/gcc/gcc-${GCC_VERSION}/gcc-${GCC_VERSION}.tar.gz --no-check-certificate

tar -xf gcc-${GCC_VERSION}.tar.gz
cd gcc-${GCC_VERSION}
./contrib/download_prerequisites

mkdir gcc-${GCC_VERSION}_build
cd   gcc-${GCC_VERSION}_build

../configure --prefix=/share/soft/gcc-${GCC_VERSION} --disable-multilib --enable-languages=c,c++
make -j 20
make -j 20   install

#Install/Activate gcc
GCC_VERSION=9.2.0
export LD_LIBRARY_PATH=/share/soft/gcc-${GCC_VERSION}/lib64:${LD_LIBRARY_PATH}
```

#### Install dnf
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
ssh g01
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

#### Install NVIDIA driver
```bash

#Driver
yum localinstall dkms-2.8.4-1.el7.noarch.rpm

distribution=rhel7
ARCH=$( /bin/arch )
yum-config-manager --add-repo http://developer.download.nvidia.com/compute/cuda/repos/$distribution/${ARCH}/cuda-$distribution.repo
yumdownloader nvidia-driver-latest-dkms 
yumdownloader nvidia-modprobe-latest-dkms nvidia-driver-latest-dkms-cuda nvidia-driver-latest-dkms-devel
yumdownloader nvidia-driver-latest-dkms-NVML nvidia-xconfig-latest-dkms nvidia-driver-latest-dkms-libs
yumdownloader nvidia-driver-latest-dkms-cuda-libs nvidia-driver-latest-dkms-NvFBCOpenGL
yumdownloader kmod-nvidia-latest-dkms nvidia-persistenced-latest-dkms

ssh g01
cd /share/src/irazu_deps/
yum localinstall -y nvidia-*.rpm kmod-nvidia-*.rpm

#CUDA
yumdownloader cuda cuda-11-5 cuda-*-11-5 *-11-5 cuda-toolkit-*-common
yumdownloader nsight-systems nsight-compute-2021.3.1 nvidia-fs
yumdownloader cuda cuda-11-5 cuda-*-11-5 *-11-5 cuda-toolkit-*-common

yumdownloader nvidia-libXNVCtrl-devel nvidia-settings nvidia-libXNVCtrl 


ssh g01
cd /share/src/irazu_deps/
yum localinstall -y cuda-*.rpm *-11-5*.rpm nvidia-*.rpm 
yum localinstall -y *-11-5*.rpm cuda-*.rpm nvidia-*.rpm 

nvidia-smi | grep NVIDIA
```

#### Install Irazu
```bash
cd /share/soft/irazu/install
./irazu-femdem-installer.sh

#Obtain license files
./pshostid

#Setup Irazu environment
export PATH=$PATH:/opt/Irazu/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/Irazu/lib:/usr/local/lib:/usr/lib:/usr/local/lib64:/usr/lib64
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/x86_64-linux-gnu/:/usr/local/lib:/usr/lib:/usr/local/lib64:/usr/lib64
export IRAZU_LICENSE_PATH=/opt/Irazu/license
```



#rt pass Passw0rd@123pg



