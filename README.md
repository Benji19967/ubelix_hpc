# Ubelix


## Setting up passwordless ssh login

https://hpc-unibe-ch.github.io/firststeps/SSH-keys/

On local machine:

```shell
ssh-copy-id <user>@submit01.unibe.ch
```

This will copy the contents of `~/.ssh/id_rsa.pub` from the local machine and append it to `~/.ssh/authorized_keys` on the remote (HPC) machine.

## Using an alias for ssh login

Instead of using 

```
ssh <user>@submit01.unibe.ch
```

add this to `~/.ssh/config` on your local machine

```
Host ubelix
    HostName submit01.unibe.ch
    User <user>
```

then you can login with

```
ssh ubelix
```

## Interacting with GitHub

First we add the content of the file `~/.ssh/id_ed25519_ubelix_internal.pub` (the public key), living on the HPC machine, to 
GitHub ssh keys as an authorized key.

By default, when performing any interactions through ssh, like cloning a repo using the ssh option, ssh might try authenticating with the 
wrong key and authentication fails. So we specify which (private) key to use for authentication. To `~/.ssh/config` on the HPC machine, add:

```
Host github.com
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519_ubelix_internal
```

Setting up git user configs:

```
git config --global user.name "<full name>"
git config --global user.email "<email linked to github account>"
```

## Requesting a GPU node

```
srun --partition=gpu --gres=gpu:rtx4090:1 --cpus-per-task=4 --nodes=1 --mem=16G --time=02:00:00 --pty bash
```

## Runing a java program

Check which java modules are available:

```
module avail Java
```

Load a java module:

```
 module load Java
```

Check the versions:

```
java --version
javac --version
```

Login to a compute machine using SLURM:

```
srun --cpus-per-task=8 --time=10:00 --pty bash
```

Run a program:

```
java <path to program>
```

## Setting up Python

### Set up `uv`

```
curl -Ls https://astral.sh/uv/install.sh | sh
```

This installs `uv` in `~/.local/bin/`.

Create some scratch space: (see https://hpc-unibe-ch.github.io/storage/scratch/) for `.venv`s. In `$SCRATCH` you have 15TB available per user, but it is not backed up.
Perfect for virtual environments.

```
module load Workspace_Home
mkdir $SCRATCH
chmod 700 $SCRATCH
```

Add this to `~/.bashrc`:

```
export UV_CACHE_DIR=$SCRATCH/uv-cache
```

### Install PyTorch with conda

TODO: do we need conda or can `uv` handle it all?

Load conda:

```
module load Anaconda3
conda --version
```

You can always check which modules are loaded in you current shell:

```
module list
```

Create a conda env:

```
conda create -n glaucoma python=3.13
```

Initialize conda: (only need to run this once, it sets up conda in `~/.bashrc`)

```
conda init
source ~/.bashrc
```

Activate the environment:

```
conda activate glaucoma
```

Load CUDA:
```
module load CUDA/12.1
nvcc --version
```

Install PyTorch:

```
conda install pytorch torchvision pytorch-cuda=12.1 -c pytorch -c nvidia
```

Install uv: (will be used for (non-GPU-related) Python package management)

```
pip install uv
```
