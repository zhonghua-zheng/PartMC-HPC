# Conda Installation

Zhonghua Zheng (zhonghua.zheng@outlook.com)

Last update: 2021/04/26

## Introduction

Here we would install Conda and create our first conda environment "partmc"

## download and activate conda

```bash
# Download and install conda
$ cd $HOME
# If you are using Campus Cluster
$ cd /projects/your_path
# note: on Campus Cluster, we need to install under the /projects directory
# e.g., cd /projects/ctessum/zzheng25
$ wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
$ chmod +x Miniconda3-latest-Linux-x86_64.sh
$ ./Miniconda3-latest-Linux-x86_64.sh

###### important ######
# Edit .bash_profile or .bashrc, add ":$HOME/miniconda3/bin"
# assume the original one is "PATH=$PATH:$HOME/bin" 
# then we would have 
# "PATH=$PATH:$HOME/bin:$HOME/miniconda3/bin"
# if we use Campus Cluster, please change it accordingly, e.g., 
# "PATH=$PATH:$HOME/bin:/projects/ctessum/zzheng25/miniconda3/bin" 
########################

# Activate the conda system, depends on which one you edited 
$ source .bash_profile
$ source .bashrc
```

## create and activate a conda environment

### option 1: install the environment manually

```bash
# assume we want to create an environment named "partmc", with Python version 3.7 

# we only need to create the environment once
$ conda create -n partmc python=3.7

# activate this conda environment 
# we would do this everytime 
$ source activate partmc

# assume we want to install some useful Python packages, e.g., numpy, pandas, scipy, scikit-learn, netcdf4, xarray, matplotlib, jupyterlab
$ conda install -c conda-forge numpy pandas scipy scikit-learn netcdf4 xarray matplotlib jupyterlab
# assume we want to install something that is not available in conda-forge
$ pip install xgboost==1.1.1
```

### option 2: install the environment using a file

create a file, rename it to be "environment.yml"

```
channels:
- conda-forge
- defaults

dependencies:
- python=3.7.0
- numpy
- pandas
- scipy
- scikit-learn
- netcdf4
- xarray
- matplotlib
- jupyterlab
- ipykernel
- ipywidgets
- pip
- pip:
  - xgboost==1.1.1
name: partmc
```

then run the commands

```bash
# Create an environment "partmc" and install the necessary packages
$ conda env create -f environment.yml
# Activate the "partmc" environment
$ source activate partmc
```

