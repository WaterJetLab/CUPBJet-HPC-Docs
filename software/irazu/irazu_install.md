
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
cd /share/src/dnf
yumdownloader epel-release
yumdownloader dnf
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
cd /share/src/dnf
rm -f *i686.rpm
yum localinstall *.rpm
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
yumdownloader nsight-systems nsight-compute-2021.3.1
yumdownloader nvidia-fs nvidia-fs-dkms
yumdownloader cuda cuda-11-5 cuda-*-11-5 *-11-5 cuda-toolkit-*-common

yumdownloader nvidia-libXNVCtrl-devel nvidia-settings nvidia-libXNVCtrl 


ssh g01
cd /share/src/irazu_deps/
yum localinstall -y cuda-*.rpm *-11-5*.rpm nvidia-*.rpm 
yum localinstall -y *-11-5*.rpm cuda-*.rpm nvidia-*.rpm nsight-*.rpm

nvidia-smi | grep NVIDIA
```

#### Install Irazu
```bash
cd /share/soft/irazu/install
./irazu-femdem-installer.sh

#Obtain license files
./pshostid

#Setup Irazu License
export IRAZU_LICENSE_PATH=/share/soft/irazu/install_g01/license/

#Testing
GCC_VERSION=9.2.0
export LD_LIBRARY_PATH=/share/soft/gcc-${GCC_VERSION}/lib64:${LD_LIBRARY_PATH}


irazu_2d --in /share/home/wangbin/test_irazu/UCS_tutorial_femdem.r2m --out /share/home/wangbin/test_irazu/output
```

#### Activate Irazu on g01
```bash
cd /share/soft/irazu/install_g01/

#make sure em1 of ethernet on g01 is connnected
ip addr show
ifdown em1
ifup em1

#check activation code based on 
./pshostid
'pshostid for lin64: 2cea7feaf8a8'

#create support file
irazu_2d --license-support

#change permission for all
chmod +x CUP-Sheng-engine-2022b.lic
chmod +x CUP-cluster-2022.clic
```



#rt pass Passw0rd@123pg



