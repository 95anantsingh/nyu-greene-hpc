# Anaconda Setup

- Conda is already available on Greene. To access the default conda environment, just type the following. (To save time in the future, you can also add this line to `~/.bashrc`)

  ```
  $ module load anaconda3/2020.07
  ```

- There's a caveat though, when you try to create a new virtual environment, it does that in the `/home` directory by default, and that takes up a lot of space. As a workaround, direct conda to store packages data in `/scratch`.

  ```
  $ conda config --append pkgs_dirs /scratch/<netid>/pkgs_dirs
  $ conda config --append envs_dirs /scratch/<netid>/envs_dirs
  ```

- Sometimes, conda might throw a `CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'`. 

  To resolve, you might want to run

  ```
  $ conda init bash
  ```

- You can now proceed to create a new virtual environment
  ```
  $ conda create -n <env_name> python=3.8
  ```