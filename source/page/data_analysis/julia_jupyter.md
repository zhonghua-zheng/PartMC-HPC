# Julia and Jupyter

Zhonghua Zheng (zhonghua.zheng@outlook.com)

Last update: 2021/04/26

## Introduction

Here we would 

- download and install Julia
- create our conda environment "julia"
- use Jupyter notebook on HPC with a GPU

## Prerequisites

Make sure you have "git" and "conda" available.

If "conda" is not available, please follow the "**Conda Installation**"

## download and Install Julia

Note here we create another environment "julia" other than "partmc".

```bash
# Download and install julia: https://julialang.org/downloads/platform/#linux_and_freebsd
$ cd $HOME
# If you are using Campus Cluster
$ cd /projects/your_path
$ wget https://julialang-s3.julialang.org/bin/linux/x64/1.6/julia-1.6.1-linux-x86_64.tar.gz
$ tar zxvf julia-1.6.1-linux-x86_64.tar.gz
$ mv julia-1.6.1 julia

###### important ######
# Edit .bash_profile or .bashrc, add ":$HOME/julia/bin"
# assume the original one is "PATH=$PATH:$HOME/bin:$HOME/miniconda3/bin" 
# then we would have 
# "PATH=$PATH:$HOME/bin:$HOME/miniconda3/bin:$HOME/julia/bin"
# if we use Campus Cluster, please change it accordingly, e.g., 
# "PATH=$PATH:$HOME/bin:/projects/ctessum/zzheng25/miniconda3/bin:/projects/ctessum/zzheng25/julia/bin" 
########################

# Activate the conda system, depends on which one you edited 
$ source .bash_profile
$ source .bashrc
```

## create our conda environment "julia"

```bash
# create conda environment and install jupyter notebook
$ conda create -n julia
$ conda activate julia
$ conda install -c conda-forge python=3.7 jupyter

# ref: https://github.com/JuliaLang/IJulia.jl
$ julia
# please change the path accordingly
julia> ENV["JUPYTER"] ="/projects/ctessum/zzheng25/miniconda3/envs/julia/bin/jupyter"
julia> using Pkg; Pkg.instantiate()
julia> Pkg.add("JLLWrappers"); Pkg.add("libsodium_jll"); Pkg.add("ZMQ")
# julia> Pkg.build("libsodium_jll"); Pkg.build("ZMQ")
julia> using JLLWrappers, libsodium_jll, ZMQ
julia> Pkg.add("IJulia")
julia> using IJulia
julia> Pkg.add("CUDA")
julia> using CUDA

$ ln -s your_julia_location/bin/julia your_conda_location/envs/julia/bin/julia
# e.g., $ ln -s /projects/ctessum/zzheng25/julia/bin/julia /projects/ctessum/zzheng25/miniconda3/envs/julia/bin/julia
```

## use Jupyter notebook on HPC with a GPU

**step 1: run the following script**

```bash
#!/bin/bash

# if use gpu:
srun --partition=ctessum --nodes=1 --time=03:00:00 --gres=gpu:QuadroRTX6000:1 --pty bash -i
# cpu only:
# srun --partition=ctessum --nodes=1 --time=03:00:00 --pty bash -i
source activate julia
echo "ssh -N -L 8880:`hostname -i`:8880 $USER@cc-login.campuscluster.illinois.edu"
jupyter notebook --port=8880 --no-browser --ip=`hostname -i`
```

**step 2: launch a new terminal, copy and paste the command printed by the "echo" command, and log in**

**step 3: open your browse (e.g., Google Chrome), type `https://localhost:8880`**

## Trouble Shooting 

GPU relevant commands

```bash
$ lspci | grep -i nvidia
$ nvidia-smi
julia > CUDA.version()
julia > CUDA.versioninfo()
julia > [CUDA.capability(dev) for dev in CUDA.devices()]
```

kill session:

```bash
$ ps -u your_netid -f | grep ssh
$ kill -9 session_id
```

for nfs:

```bash
$ lsof | grep nfs00000
$ kill -9 session_id
```
