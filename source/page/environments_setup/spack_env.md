# spack: HPC Packages and Compliers Installation

Zhonghua Zheng (zhonghua.zheng@outlook.com)

Last update: 2021/04/26

## Introduction

Below is an example of installing "gcc", "hdf5", "netcdf", "cmake"

## Prerequisites

Make sure you have "git" and "Python" available.

If "Python" is not available, please follow the "**Conda Installation**"

## option 1: install from a script

```bash
$ mkdir /projects/your_path
$ module load git 
$ module load python
# if you have followed the "Conda Installation"
# $ source activate partmc
$ git clone https://github.com/spack/spack.git
```

### 1.1 create a script

please modify the following commands appropriately with your partition, netid and path

- `#SBATCH --partition=xxx` -> `#SBATCH --partition=ctessum`
- `#SBATCH --mail-user=your_netid@illinois.edu` 
- `export SPACK_ROOT=/projects/your_path/spack`

```bash
#!/bin/bash

#SBATCH --partition=xxx
#SBATCH --nodes=1
#SBATCH --mem=64g
#SBATCH --time=12:00:00
#SBATCH --job-name=spack_install
#SBATCH --mail-type=ALL
#SBATCH --mail-user=your_netid@illinois.edu

export SPACK_ROOT=/projects/your_path/spack
source ${SPACK_ROOT}/share/spack/setup-env.sh
spack install gcc@9.3.0 %gcc@4.8.5 target=x86_64
spack load gcc@9.3.0
spack compiler find
spack install hdf5%gcc@9.3.0 target=x86_64 +cxx+fortran+hl+pic+shared+threadsafe
spack load hdf5%gcc@9.3.0
spack install netcdf-fortran%gcc@9.3.0 target=x86_64 ^hdf5+cxx+fortran+hl+pic+shared+threadsafe
spack load netcdf-fortran
spack load netcdf-c
spack install cmake%gcc@9.3.0 target=x86_64
```

### 1.2 Run the command

assume the name of the script is "install_spack.sh"

```bash
$ sbatch install_spack.sh
```

- took ~8 hours

## option 2: install it manually

Please use the **compute note** (recommand 8 hrs):

```bash
$ srun --partition=xxx --nodes=1 --mem=64g  --time=08:00:00 --pty bash -i
# e.g., srun --partition=ctessum --nodes=1 --mem=64g  --time=08:00:00 --pty bash -i
$ mkdir /projects/your_path
$ module load git 
$ module load python
# if you have followed the "Conda Installation"
# $ source activate partmc
$ git clone https://github.com/spack/spack.git
$ export SPACK_ROOT=/projects/your_path/spack
$ source ${SPACK_ROOT}/share/spack/setup-env.sh
# use gcc@4.8.5 to build gcc@9.3.0
$ spack install gcc@9.3.0 %gcc@4.8.5 target=x86_64
$ spack load gcc@9.3.0
$ spack compiler find
$ spack install hdf5%gcc@9.3.0 target=x86_64 +cxx+fortran+hl+pic+shared+threadsafe
$ spack load hdf5%gcc@9.3.0
$ spack install netcdf-fortran%gcc@9.3.0 target=x86_64 ^hdf5+cxx+fortran+hl+pic+shared+threadsafe
$ spack load netcdf-fortran
$ spack load netcdf-c 
$ spack install cmake%gcc@9.3.0 target=x86_64
```

## useful commands and scripts

**commands**

```bash
# find location
$ spack location -i gcc@9.3.0
# add new compiler
$ spack compiler find
# Spack compilers should print out a list of available compilers 
$ spack compilers
# Spack will print out a long list of info. 
$ spack config get compilers
```

**scripts**

Below is the script to load the spack environment. Please lease change the path accordingly

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

## Reference

GEOS-Chem: [tutorial](https://gchp.readthedocs.io/en/latest/supplement/spack.html), [GitHub discussion](https://github.com/geoschem/geos-chem/issues/637) (for full installation including cdo)

Chinese: [link](https://re-ra.xyz/Spack-%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97/#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%86%99%E6%9C%AC%E6%96%87)

