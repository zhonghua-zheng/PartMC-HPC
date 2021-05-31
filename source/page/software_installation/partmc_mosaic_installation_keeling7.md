PartMC-MOSAIC Installation on keeling
======

Zhonghua Zheng and [Jeffrey Curtis](https://publish.illinois.edu/jcurtis2/) (jcurtis2 at illinois.edu)

Last update: 2021/05/30

## Prerequisites

Make sure you have access to keeling

Make sure you to have the permission to use the MOSAIC software

## access keeling and prepare install

```bash
# from your local machine
$ ssh -y your_NetID@keeling7.earth.illinois.edu
# launch an interactive job
$ qsub -I -l select=1:ncpus=32 -l walltime=24:00:00
$ module load gnu/gnu-9.3.0
$ module load gnu/netcdf4-4.7.4-gnu-9.3.0
$ module load gnu/openmpi-3.1.6-gnu-9.3.0
$ export FC=gfortran
$ export CC=gcc
```

You can check if you have loaded successfully by typing

```bash
$ module list
```

You can check if you have set up `FC` and `CC` successfully by typing

```bash
$ which $FC
$ which $CC
```

## **build MOSAIC chemistry** 

```bash
$ cd your_path
$ tar -zxvf mosaic-2012-01-25.tar.gz
$ mv mosaic-2012-01-25 mosaic
$ cd your_path/mosaic
$ cp Makefile.local.gfortran Makefile.local
$ make
```

## build PartMC-MOSAIC

step 1: download PartMC and set up 

```bash
$ cd your_path
$ module load git
$ git clone git@github.com:compdyn/partmc.git
$ cd partmc
$ mkdir build
$ cd build
$ export NETCDF_HOME=/sw/netcdf4-4.7.4-gnu-9.3.0
$ export MOSAIC_HOME=your_path/mosaic
$ ccmake ..
```

step 2: First, press "c". Then press "e", and type the following options

```
CMAKE_BUILD_TYPE: RELEASE

ENABLE_MOSAIC: ON 

NETCDF_C_LIB: 
/sw/netcdf4-4.7.4-gnu-9.3.0/lib/libnetcdf.so

NETCDF_FORTRAN_LIB: 
/sw/netcdf4-4.7.4-gnu-9.3.0/lib/libnetcdff.so

NETCDF_INCLUDE_DIR: 
/sw/netcdf4-4.7.4-gnu-9.3.0/include
```

step 3: press "c", then "c" again, and "g"

step 4: compile and make sure you have all the test cases such as "test_mosaic_1" and "test_mosaic_2" passed.

```bash
make
make test
```

