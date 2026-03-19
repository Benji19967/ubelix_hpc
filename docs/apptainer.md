# Apptainer

docs: https://apptainer.org/docs/user/1.0/quick_start.html#interact-with-images

Pull an image:

```
apptainer pull docker://alpine
```

```
apptainer pull docker://sylabsio/lolcow
```

List images:

```
apptainer cache list -v
```

Interacting with an image:

```
apptainer shell lolcow_latest.sif
```

Now you can run commands inside the container, ex. check the operating system:

```
Apptainer> cat /etc/*release
```

Check user and id configs:

```
Apptainer> whoami
Apptainer> id
```

Executing a command:

```
apptainer exec lolcow_latest.sif cowsay moo
```

Running a container:

```
apptainer run lolcow_latest.sif
```

or 

```
./lolcow_latest.sif
```

Passing arguments to `run`:

```
apptainer run docker://alpine echo "hello"
```

Working with a file:

```
apptainer exec lolcow_latest.sif cat $HOME/.bashrc
```

By default Apptainer bind mounts `/home/$USER`, `/tmp`, and `$PWD` into your container at runtime.

Building custom apptainer:

Create a definition file, for example `lolcow.def`, then

```
apptainer build lolcow.sif lolcow.def
```
