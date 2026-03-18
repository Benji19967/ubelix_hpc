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
