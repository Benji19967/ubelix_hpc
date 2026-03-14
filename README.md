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
