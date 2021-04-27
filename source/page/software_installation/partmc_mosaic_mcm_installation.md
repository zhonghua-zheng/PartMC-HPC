PartMC-MOSAIC-MCM Installation
======

Zhonghua Zheng (zhonghua.zheng@outlook.com) and Xiaokai Yang

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

## **download MOSAIC-MCM** 

run the following command

```bash
$ cd /projects/your_path
$ tar -zxvf mosaic-2012-01-25.tar.gz
$ tar -zxvf PartMC-MOSAIC-MCMv3.3.1.tar.gz
$ mv PartMC-MOSAIC-MCMv3.3.1 partmc-mcm
```

## build MOSAIC-MCM

Create a script ```mcm_compile.sh```  to compile MOSAIC-MCM. 

Modify your ```partition```, ```job-name```, and  ```mail-user``` properly.

```sh
#!/bin/bash

#SBATCH --partition=ctessum
#SBATCH --nodes=1
#SBATCH --mem=64g
#SBATCH --time=2:00:00
#SBATCH --job-name=mcm_compile
#SBATCH --mail-type=ALL
#SBATCH --mail-user=xxxxxx@illinois.edu

cd /projects/ctessum/zzheng25/partmc-mcm/MOSAIC-MCM
make
```

Submit your script. It would take around 0.5 hrs.

``` bash
sbatch mcm_compile.sh
```

## build PartMC-MOSAIC-MCM

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
######## important ########
# cd /projects/your_path/partmc/src
# modify the lines, have 1000 -> 10000
# call assert(719237193, n_spec < 1000)
# character(len=((GAS_NAME_LEN + 2) * 1000)) :: gas_species_names
###########################
$ cd /projects/your_path/partmc/
$ mkdir build
$ cd build
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

MOSAIC_INCLUDE_DIR:
/projects/ctessum/zzheng25/partmc-mcm/MOSAIC-MCM/datamodules

MOSAIC_LIB:
/projects/ctessum/zzheng25/partmc-mcm/MOSAIC-MCM/libmosaic.a
```

step 4: press "c", then "c" again, and "g"

step 5: compile and make sure you have all the test cases passed, except for "test 48" and "test 50"

```bash
make
make test
```
