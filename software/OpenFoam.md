
# Download OpenFOAM7
```bash
cd $HOME
mkdir OpenFOAM
cd OpenFOAM

# Donwload these from PC with VPN
git clone https://github.com/OpenFOAM/OpenFOAM-7.git
git clone https://github.com/OpenFOAM/ThirdParty-7.git

unzip OpenFOAM-7-master.zip OpenFOAM-7
unzip ThirdParty-7-master ThirdParty-7
```

# Install compiling environment
```bash

#Go to $HOME/OpenFOAM/OpenFOAM-7/etc/bashrc
#Set WM_LABEL_SIZE=64 FOAMY_HEX_MESH=yes

#Go to $HOME/.bashrc

# Build necessary parts if not available

cd $HOME/ThirdParty-7
mkdir download
wget -P download https://www.open-mpi.org/software/ompi/v2.1/downloads/openmpi-2.1.1.tar.bz2
tar -xjf download/openmpi-2.1.1.tar.bz2

#https://www.open-mpi.org/faq/?category=building
cd openmpi-2.1.1
mkdir build
./configure --prefix=$HOME/openmpi-2.1.1/

make all -j 10 install

export OPENMPI_DIR=$HOME/openmpi-2.1.1/

export PATH=\$PATH:\$OPENMPI_DIR/bin
export LD_LIBRARY_PATH=\$LD_LIBRARY_PATH:\$OPENMPI_DIR/lib
```
