# Anaconda Guide

## One Time Setup
Conda is already available on Greene. To access the default conda environment, just type the following. (To save time in the future, you can also add this line to `~/.bashrc`)

```bash
module load anaconda3/2020.07
```

There's a caveat though, when you try to create a new virtual environment, it does that in the `/home` directory by default, and that takes up a lot of space out of your quota. As a workaround, direct conda to store packages data in `/scratch`.

```bash
conda config --append pkgs_dirs /scratch/<netid>/conda/pkgs_dirs
conda config --append envs_dirs /scratch/<netid>/conda/envs_dirs
```

Sometimes, conda might throw a `CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'`. 

To resolve this, you might want to run

```bash
conda init bash
```

You can now proceed to create a new virtual environment
```bash
conda create -n <env_name> python=3.8
```

```{tip}
The number of files quota usually gets filled up very quickly, if you use conda/pip environment as they generate a large number of residual files. They don’t remove them automatically and hence stay in your $HOME affecting your files quota.

We recommend you to clean your conda/pip cache files (if any). This cleaning up of cache files is very easy and needs only a single command to be run.

Following commands and links can come handy:

- `conda clean -a`
- `pip cache purge`
```


  ## Cheatsheet

