# Anaconda

(conda-setup)=
## One Time Setup

Conda is already available on Greene. To access the default conda environment, just type the following.

```bash
module load anaconda3/2020.07
```

There's a caveat though, when you try to create a new virtual environment, it does that in the `/home` directory by default, and that takes up a lot of space out of your quota. As a workaround, direct conda to store packages data at `/scratch`.

```bash
conda config --append pkgs_dirs /scratch/<netid>/conda/pkgs_dirs
conda config --append envs_dirs /scratch/<netid>/conda/envs_dirs
```

````{admonition} Common Error
:class: error

Sometimes, conda might throw an error:

> `CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'`. 

To resolve this, you might want to run

```bash
conda init bash
```
````

## Activate conda

You need to make conda accessible from you shell by running the followind command:

```bash
module load anaconda3/2020.07
eval "$(conda shell.bash hook)"
```

You can now proceed to create a new virtual environment
```bash
conda create -n <env_name> python=3.8
```

or activate existing environment

```bash
conda activate <env_name>
```

````{tip}
You can add these two lines to your `~/.bashrc` file for faster access
```bash
echo 'module load anaconda3/2020.07'   >> ~/.bashrc
echo 'eval "$(conda shell.bash hook)"' >> ~/.bashrc
```
````

```{admonition} Recommended
:class: hint

The number of files quota usually gets filled up very quickly, if you use conda/pip environment as they generate a large number of residual files. They don’t remove them automatically and hence stay in your $HOME affecting your files quota.

We recommend you to clean your conda/pip cache files (if any). This cleaning up of cache files is very easy and needs only a single command to be run.

Following commands and links can come handy:

- `conda clean -a`
- `pip cache purge`
```

## Cheatsheet

Most used conda commands are listed below for quick reference.

### Quick Start

| Command                                         | Description                                       |
|-------------------------------------------------|---------------------------------------------------|
| `conda info`                                    | verify conda install and check version            |
| `conda create --name ENV-NAME`                  | create a new environment                          |
| ` conda create -n ENV-NAME python=3.10`         | create environment with Python version            |
| `conda activate ENV-NAME`                       | activate environent                               |
| `conda activate base`                           | reactivate base environment                       |

### Package Management

| Command                                         | Description                                       |
|-------------------------------------------------|---------------------------------------------------|
| `conda install PKG1 PKG2...`                    | install packages                                  |
| `conda install -c CHANNEL-NAME PKG1`            | install packages from specified channel           |
| `conda install PKG=3.1.4`                       | install specific version of package               |
| `conda uninstall PKG-NAME`                      | uninstall package                                 |
| `conda list`                                    | list installed packages                           |
| `conda update --all`                            | update all packages of current environment        |

### Environment Management

| Command                                         | Description                                       |
|-------------------------------------------------|---------------------------------------------------|
| `conda info --envs`                             | list all environments and locations               |
| `conda create --clone ENV-NAME -n NEW-ENV`      | clone environment                                 | 
| `conda list --revisions`                        | list revisions made to environment                |
| `conda install --revision NUMBER`               | restore environment to a revision                 |
| `conda remove -n ENV-NAME --all`                | delete environment by name                        |
| `conda env export --from-history > FILE.yml`    | export cross-platform compatible environment      |
| `conda env export ENV-NAME > FILE.yml`          | export platform and package specific environment  |
| `conda env create -n ENV-NAME --file FILE.yml`  | import environment from a .yml file               |

### Maintenance

| Command                                         | Description                                       |
|-------------------------------------------------|---------------------------------------------------|
| `conda clean --all`                             | remove all unused files                           |
| `conda config --show`                           | examine conda configuration                       |


```{note}
A more comprehensive documentation is available [here](https://docs.conda.io/projects/conda/en/latest/user-guide/index.html).
```