# Job Management

## Quick Start Guide

## Batch Job

## Interactive Job
Instead of submitting a job one can request an interactive session on NYU Greene where you can access th shell and run as many jobs as the resources allows you.

Request a session with 4 CPU cores
```bash
srun -c 4  --pty /bin/bash
```

Request a GPU session with 32 GB Ram and 10 CPU cores for 1 hour
```bash
srun -c 10 --mem=32GB --gres=gpu:1 -t 1:00:00 --pty /bin/bash
```

The default login node does not have a GPU, and is not recommended to run compute heavy jobs. So if you want to run compute heavy commands in real-time, you can submit an interacve job instead.

The following command allocates a GPU for 1 hour with 32GB of RAM, and X11 forwarding enabled. (Note that the `--x11` flag requires `ssh -X` as menoned in the previous step for X11 forwarding to work)

```
$ srun --x11 --gres=gpu:1 --mem=32000 --time=1:00:00 --pty /bin/bash
```

## Job Array

## Submitting a Job

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

## Checking Job Status

## Cancelling a Job