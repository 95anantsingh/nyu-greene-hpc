# Interactive Sessions

Instead of submitting a job one can request an interactive session on NYU Greene where you can access th shell and run as many jobs as the resources allows you.

## Get Interactive session

Request a session with 4 CPU cores
```bash
srun -c 4  --pty /bin/bash
```

Request a GPU session with 32 GB Ram and 10 CPU cores for 1 hour
```bash
srun -c 10 --mem=32GB --gres=gpu:1 -t 1:00:00 --pty /bin/bash
```


```bash

```

```bash

```

You can follow the below steps to set up your environments on Greene easily.

## Anaconda on Greene

- Conda is already available on Greene. To access the default conda environment, just type the following. (To save time in the future, you can also add this line to `~/.bashrc`)

  ```
  $ module load anaconda3/2020.07
  ```

- There's a caveat though, when you try to create a new virtual environment, it does that in the `/home` directory by default, and that takes up a lot of space. As a workaround, direct conda to store packages data in `/scratch`.

  ```
  $ conda config --append pkgs_dirs /scratch/<netid>/pkgs_dirs
  $ conda config --append envs_dirs /scratch/<netid>/envs_dirs
  ```

- Sometimes, conda might throw a `CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.`. To resolve, you might want to run

  ```
  $ conda init bash
  ```

- You can now proceed to create a new virtual environment
  ```
  $ conda create -n <env_name> python=3.8
  ```

---

## X11 Forwarding

You might want to render the environment and see the GUI on your local machine. If you have a Mac, the easiest way is to install XQuartz. Make sure to restart the system after installaon.

```
$ brew install --cask xquartz
```

To enable X11 forwarding, instead of regular SSH, use

```
$ ssh -X <netid>@greene.hpc.nyu.edu
```

---

## Submitting Jobs

### Using SLURM

Greene uses SLURM to submit jobs. You can check the sample script below as a reference to submit your jobs.

```
#!/bin/bash
#SBATCH --job-name=training
#SBATCH --nodes=1
#SBATCH --cpus-per-task=4
#SBATCH --output=%x.out
#SBATCH --mem=16GB
#SBATCH --time=4:00:00
#SBATCH --gres=gpu:1

module purge
module load anaconda3/2020.07
eval "$(conda shell.bash hook)"
conda activate env_name
cd <path_to_your_directory>

python <your_script>.py
```

The script above will submit a job with a maximum time of 4 hours on a node with 4 CPUs, 16 GB RAM and 1 GPU. The GPU can be either RTX8000 or V100. If you want to select a specific GPU, you can specify that by `gpu:rtx8000:1` and `gpu:v100:1`.

### Interactive Jobs

The default login node does not have a GPU, and is not recommended to run compute heavy jobs. So if you want to run compute heavy commands in real-time, you can submit an interacve job instead.

The following command allocates a GPU for 1 hour with 32GB of RAM, and X11 forwarding enabled. (Note that the `--x11` flag requires `ssh -X` as menoned in the previous step for X11 forwarding to work)

```
$ srun --x11 --gres=gpu:1 --mem=32000 --time=1:00:00 --pty /bin/bash
```

