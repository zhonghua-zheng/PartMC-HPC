# Python and Jupyter

Zhonghua Zheng (zhonghua.zheng@outlook.com)

Last update: 2021/04/26

## Introduction

Here we would 

- use Jupyter notebook on HPC with a GPU

## Prerequisites

Make sure you have a conda environment ("partmc") available.

If the "partmc" conda environment is not available, please follow the "**Conda Installation**"

## use Jupyter notebook on HPC with a GPU

**step 1: run the following script**

Please change the partition and gpu configuration accordingly

```bash
#!/bin/bash

# if use gpu:
srun --partition=ctessum --nodes=1 --time=03:00:00 --gres=gpu:QuadroRTX6000:1 --pty bash -i
# cpu only:
# srun --partition=ctessum --nodes=1 --time=03:00:00 --pty bash -i
source activate partmc
echo "ssh -N -L 8880:`hostname -i`:8880 $USER@cc-login.campuscluster.illinois.edu"
jupyter notebook --port=8880 --no-browser --ip=`hostname -i`
```

**step 2: launch a new terminal, copy and paste the command printed by the "echo" command, and log in**

**step 3: open your browse (e.g., Google Chrome), type `https://localhost:8880`**

## Trouble Shooting 

GPU relevant command

```bash
$ lspci | grep -i nvidia
$ nvidia-smi
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

