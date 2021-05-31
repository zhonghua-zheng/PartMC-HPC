PartMC-MOSAIC-MCM Installation on keeling
======

Zhonghua Zheng (zhonghua.zheng@outlook.com) 

Last update: 2021/05/30

## Prerequisites

Make sure you have access to 

- keeling7
- github.com:xiaoky97/MCM-PartMC-MOSAIC

## load environment

```bash
$ qsub -I -l select=1:ncpus=32 -l walltime=24:00:00
$ module load gnu/gnu-9.3.0
$ module load gnu/netcdf4-4.7.4-gnu-9.3.0
$ module load gnu/openmpi-3.1.6-gnu-9.3.0
$ export FC=gfortran
$ export CC=gcc
```

## **download MOSAIC-MCM** 

run the following command

```bash
$ cd ~
$ git clone git@github.com:xiaoky97/MCM-PartMC-MOSAIC.git
```

## build MOSAIC-MCM

```sh
$ cd ~/MCM-PartMC-MOSAIC/MOSAIC-MCM
make
```

It takes ~30 mins.

## build PartMC-MOSAIC-MCM

step 1: set up 

```bash
$ export NETCDF_HOME=/sw/netcdf4-4.7.4-gnu-9.3.0
$ cd ~/MCM-PartMC-MOSAIC/PartMC
$ export NETCDF_HOME=/sw/netcdf4-4.7.4-gnu-9.3.0
$ mkdir build
$ cd build
$ ccmake ..
```

step 2: First, press "c". Then press "e", and type the following options (use the paths you got from step 1):

```
CMAKE_BUILD_TYPE: RELEASE

ENABLE_MOSAIC: ON 

NETCDF_C_LIB: 
/sw/netcdf4-4.7.4-gnu-9.3.0/lib/libnetcdf.so

NETCDF_FORTRAN_LIB: 
/sw/netcdf4-4.7.4-gnu-9.3.0/lib/libnetcdff.so

NETCDF_INCLUDE_DIR: 
/sw/netcdf4-4.7.4-gnu-9.3.0/include

MOSAIC_INCLUDE_DIR (using your MOSAIC-MCM path):
/data/keeling/a/zzheng25/MCM-PartMC-MOSAIC/MOSAIC-MCM/datamodules

MOSAIC_LIB (using your MOSAIC-MCM path):
/data/keeling/a/zzheng25/MCM-PartMC-MOSAIC/MOSAIC-MCM/libmosaic.a
```

step 3: press "c", then "c" again, and "g"

step 4: compile and make sure you have all the test cases passed, except for "test 48" and "test 50"

```bash
make
make test
```
