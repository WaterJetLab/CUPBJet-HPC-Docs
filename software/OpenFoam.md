
## Download OpenFOAM7
```bash
cd $HOME
mkdir OpenFOAM
cd OpenFOAM

# Donwload these from PC with VPN
git clone https://github.com/OpenFOAM/OpenFOAM-7.git
git clone https://github.com/OpenFOAM/ThirdParty-7.git

unzip OpenFOAM-7-master.zip
mv OpenFOAM-7-master OpenFOAM-7
unzip ThirdParty-7-master.zip
mv ThirdParty-7-master ThirdParty-7
```

## Install compiling environment
```bash

# Build necessary parts if not available

cd $HOME/ThirdParty-7
mkdir download
wget -P download https://www.open-mpi.org/software/ompi/v2.1/downloads/openmpi-2.1.1.tar.bz2
wget -P download https://sourceforge.net/projects/boost/files/boost/1.55.0/boost_1_55_0.tar.bz2

tar -xjf download/openmpi-2.1.1.tar.bz2
tar -xjf download/boost_1_55_0.tar.bz2

###########
# OPENMPI #
###########
#https://www.open-mpi.org/faq/?category=building
cd openmpi-2.1.1
mkdir build
./configure --prefix=$HOME/openmpi-2.1.1/

make all -j 10 install

export OPENMPI_DIR=$HOME/openmpi-2.1.1/

export PATH=$PATH:$OPENMPI_DIR/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$OPENMPI_DIR/lib
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$OPENMPI_DIR/lib
```

## Install OpenFoam
```bash
sed -i -e 's/\(boost_version=\)boost-system/\1boost_1_55_0/' OpenFOAM-7/etc/config.sh/CGAL
sed -i -e 's/\(cgal_version=\)cgal-system/\1CGAL-4.10/' OpenFOAM-7/etc/config.sh/CGAL

#Setup building parameters
source $HOME/OpenFOAM/OpenFOAM-7/etc/bashrc WM_COMPILER_TYPE=system WM_COMPILER=Gcc48 WM_LABEL_SIZE=64 WM_MPLIB=OPENMPI FOAMY_HEX_MESH=no

#For faster compile using 16 cores in SuperMike II
export WM_NCOMPPROCS=8

#Then save an alias in the personal .bashrc file, simply by running the following command:
echo "alias of7='source \$HOME/OpenFOAM/OpenFOAM-7/etc/bashrc $FOAM_SETTINGS'" >> $HOME/.bashrc


#------!!!Build OpenFoam!!!--------
cd $WM_PROJECT_DIR
./Allwmake -j $WM_NCOMPPROCS > log.make 2>&1

#------!!!Testing!!!--------
of7
icoFoam -help
```

## Install 3rdParty Library
```bash

#hybridPorousInterFoam
# Donwload these from PC with VPN
git clone https://github.com/Franjcf/hybridPorousInterFoam.git

cd $HOME/OpenFOAM
unzip hybridPorousInterFoam-master.zip
mv hybridPorousInterFoam-master hybridPorousInterFoam

cd hybridPorousInterFoam/OpenFoamV7

#build hybridPorousInterFoam
export WM_NCOMPPROCS=8
./Allwmake -j $WM_NCOMPPROCS > log.make 2>&1
```
