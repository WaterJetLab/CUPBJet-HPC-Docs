
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

#########
# Boost #
#########
cd $FENICS_ROOT/deps

#build b2 builder
cd boost_1_60_0/tools/build
./bootstrap.sh --with-toolset=intel-linux
./b2 install --prefix=$FENICS_ROOT/deps/boost_1_60_0
export PATH=$FENICS_ROOT/deps/boost_1_60_0/bin:${PATH}

#build boost library using the builder
cd $FENICS_ROOT/deps/boost_1_60_0
b2 -j 24 \
   --with-filesystem \
   --with-iostreams \
   --with-math \
   --with-program_options \
   --with-system \
   --with-thread \
   --with-timer \
   --with-regex \
   --build-dir=boost-build \
      toolset=intel \
      stage
export BOOST_DIR=$FENICS_ROOT/deps/boost_1_60_0/
export BOOST_ROOT=$FENICS_ROOT/deps/boost_1_60_0/

sed -i -e 's/\(boost_version=\)boost-system/\1boost_1_55_0/' OpenFOAM-7/etc/config.sh/CGAL
sed -i -e 's/\(cgal_version=\)cgal-system/\1CGAL-4.10/' OpenFOAM-7/etc/config.sh/CGAL

```


