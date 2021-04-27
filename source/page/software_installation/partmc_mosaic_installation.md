PartMC-MOSAIC Installation
======

Zhonghua Zheng (zhonghua.zheng@outlook.com)

Last update: 2021/04/26

## Prerequisites

Make sure you have "git" and "spack" available.

- If "spack" is not available, please follow the "**spack: HPC Packages and Compliers Installation**"

Make sure you to have the permission to use the MOSAIC software

## load spack environment

run the following script, please use your own `SPACK_ROOT` path

```bash
#!/bin/bash

export SPACK_ROOT=/projects/ctessum/zzheng25/spack
source ${SPACK_ROOT}/share/spack/setup-env.sh
spack load gcc@9.3.0
spack compiler find
spack compilers
spack config get compilers
spack load hdf5%gcc@9.3.0
spack load netcdf-fortran
spack load netcdf-c
spack load cmake
export CC=gcc
export FC=gfortran
which gcc
```

## **build MOSAIC chemistry** 

```bash
$ cd /projects/your_path
$ tar -zxvf mosaic-2012-01-25.tar.gz
$ mv mosaic-2012-01-25 mosaic
$ cd /projects/your_path/mosaic
$ cp Makefile.local.gfortran Makefile.local
$ make
```

## build PartMC-MOSAIC

step 1: use the following commands to get the paths for later use (step 3)

```bash
$ spack location -i netcdf-fortran
$ spack location -i netcdf-c
$ spack location -i cmake
```

step 2: run the following commands

```bash
$ cd /projects/your_path
$ module load git
$ git clone git@github.com:compdyn/partmc.git
$ cd partmc
$ mkdir build
$ cd build
$ export MOSAIC_HOME=/projects/your_path/mosaic
$ ccmake ..
```

step 3: First, press "c". Then press "e", and type the following options (use the paths you got from step 1):

```
CMAKE_BUILD_TYPE: RELEASE

cmake_install_PREFIX: 
/projects/your_path/spack/opt/spack/linux-centos7-x86_64/gcc-9.3.0/cmake-XXXXXX/bin

ENABLE_MOSAIC: ON 

NETCDF_C_LIB: 
/projects/ctessum/zzheng25/spack/opt/spack/linux-centos7-x86_64/gcc-9.3.0/netcdf-c-XXXXXX/lib/libnetcdf.a 

NETCDF_FORTRAN_LIB: 
/projects/ctessum/zzheng25/spack/opt/spack/linux-centos7-x86_64/gcc-9.3.0/netcdf-fortran-XXXXXX/lib/libnetcdff.a 

NETCDF_INCLUDE_DIR: 
/projects/ctessum/zzheng25/spack/opt/spack/linux-centos7-x86_64/gcc-9.3.0/netcdf-fortran-XXXXXX/include
```

step 4: press "c", then "c" again, and "g"

step 5: compile and make sure you have all the test cases such as "test_mosaic_1" and "test_mosaic_2" passed.

```bash
make
make test
```

