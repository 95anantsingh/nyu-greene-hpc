# Job Management

On HPC, you don't run your program on the shell directly (on login nodes). Instead you submit your program as a job to run on compute nodes. These jobs are managed by SLURM and will be queued to the system and executed eventually. Conceptually, each job is a 2-step process:

1. You request certain resources from the system. The most common resources are CPU cores.
2. With the assigned resources, you run your computational tasks.

HPC provides flexibility in types of jobs as per the resource and computer requirements of its users. Down below are the most commonly used job types.


```{admonition} SLURM Useful Links
:class: hint

- [https://slurm.schedmd.com/quickstart.html](https://slurm.schedmd.com/quickstart.html)
- [https://slurm.schedmd.com/tutorials.html](https://slurm.schedmd.com/tutorials.html)
- [https://www.youtube.com/watch?v=U42qlYkzP9k&feature=player_embedded](https://www.youtube.com/watch?v=U42qlYkzP9k&feature=player_embedded)
```

## Batch Jobs

Users should use batch jobs for the most part unless your requirements can't be met without a direct shell access.<br>
A complete batch job workflow:

1. Write a job script, which consists of 2 parts:
    1. Resources requirement.
    1. Commands to be executed.
1. Submit the job.
1. Relax, have a coffee, log off if you wish. The computer will do the work.
1. Come back to examine the result.


### Batch Job Script

A job script is a text file describing the job. As discussed, the first part tells how much resources you want. The second part is what you want to run.


**Points to be noted:**
- Request only what you need
- Serial jobs would need only one CPU (#SBATCH -n 1)
- Make sure the walltime specified is not greater than the allowed time limit.

```{admonition} Difference between CPUs, Cores and Tasks
:class: note

- On Greene HPC, One CPU is equivalent to one Core.
- In Slurm, the resources (CPUs) are allocated in terms of tasks which are denoted by `-n` or `--natsks`.
- By Default, the value of `-n` or `--ntasks` is one if left undefined.
- By Default, Each task is equivalent to one CPU.
- But if you have defined `-c` or `--cpus-per-task` in your job script, then the total number of CPUs allocated to you would be the multiple of `-n` and `-c`.
```

#### Syntax

```bash
#!/bin/bash

# Define the resource requirements here using #SBATCH tag, 
# ('#' before SBATCH is required and you do not remove it).

#------ Resource requiremenmt commands start here

#SBATCH <option> <value>
#SBATCH <option> <value>
#SBATCH <option> <value>
...

#------ Resource requiremenmt commands end here

#------ Commmands to be executed

<command executable on shell>
<command executable on shell>
<command executable on shell>
...

```

Save the job scripts with `.sbatch` file extension.

#### Options

The options tell SLURM information about the job, such as what resources will be needed. These can be specified in the job-script as SBATCH directives, or on the command line as options while submitting a job, or both (in which case the command line options take precedence should the two contradict each other). 

For each option there is a corresponding SBATCH directive with the syntax:

```bash
#!/bin/bash
#SBATCH --nodes=2
```
```bash
sbatch abc.sbatch
```
or as a command-line option to sbatch when you submit the job: 
```bash
sbatch --nodes=2 abc.sbatch
```

Available options:

| Option                        | Description                                                                                                   |
|-------------------------------|---------------------------------------------------------------------------------------------------------------|
| `-J`, `--job-name=<str>`      | Give the job a name. The default is the filename of the job script. Within the job, $SBATCH_JOB_NAME <br> expands to the job name                                                                                                                         |
| `-o`, `--output <path>`       | Send stdout to path/for/stdout. The default filename is slurm-${SLURM_JOB_ID}.out, e.g. slurm-12345.out, <br> in the  directory from which the job was mitted                                                                                                 |
| `-e <path>`                   | Send stderr to specified path                                                                                 |
| `--mail-user=<email>`         | Send email to specified email id when certain events occur                                                    |
| `--mail-type=<type>`          | Valid type values are NONE, BEGIN, END, FAIL, REQUEUE, ALL...                                                 |
| `--export=[VARS]`             | Pass variables to the job, either with a specific value (the VAR= form) or from the submittingenvironment <br>  (without "=")                                                                                                                                   |
| `-t`, `--time=<time>`         | Set a limit on the total run time. Acceptable formats include  "minutes", "minutes:seconds", <br> "hours:minutes:seconds",  "days-hours", "days-hours:minutes" and "days-hours:minutes:seconds"                                                   |
| `--mem=<int><unit>`           | Maximum memory per node the job will need, unit can be MB, GB, ex- 3GB                                        |
| `--mem-per-cpu=<int><unit>`   | Memory required per allocated CPU, unit can be MB, GB                                                         |
| `-N`, `--nodes=<int>`         | Number of nodes are required. Default is 1 node                                                               |
| `-n`, `--ntasks=<int>`        | Specify the number of tasks to run, e.g. -n4. Default is one CPU core per task. Don't just submitthe job,<br>  but also wait for it to start and connect stdout, stderr and stdin to the current terminal, default is one task <br> per node                   |
| `--ntasks-per-node=<int>`     | Request that ntasks be invoked on each node                                                                   |
| `-c`, `--cpus-per-task=<int>` | Require ncpus number of CPU cores per task. Without this option, allocate one core per task                   |
| `--pty`                       | Execute the first task in pseudo terminal mode, e.g. --pty /bin/bash, to start a bash command shell           |
| `--x11`                       | Enable X forwarding, so programs using a GUI can be used during the session (provided you have X <br>forwarding to your workstation set up)                                                                                                                     |
| `--begin=<time>`              | Delay starting this job until after the specified date and time, e.g. --begin=9:42:00, to start the job at  <br>9:42:00 am                                                                                                                                  |
| `-a`, `--array=[indexes]`     | Submit an array of jobs with array ids as specified. Array ids can be specified as a numerical range, a  <br>comma-separated list of numbers, or as some combination of the two. Each job instance will have an<br> environment variable SLURM_ARRAY_JOB_ID and SLURM_ARRAY_TASK_ID.                                                                                                     |


```{note}
A more comprehensive list of options can be found [here](https://slurm.schedmd.com/sbatch.html).
```

### Submitting a batch Job

To submit a job you need to use `sbatch` command.

```bash
# sbatch [options] <filename>
sbatch abc.sbatch
```

```{attention} 
Requesting the resources you need, as accurately as possible, allows your job to be started at the earliest opportunity as well as helping the system to schedule work efficiently to everyone's benefit.
```

### Reading Outputs

After a job execution finishes, a file is generated with the name specified in sbatch file which contains the **stdout** or the shell output. You can read the output by opening the output file. 

```bash
cat job_1323234.out
```

### Examples

**Job with one 1 core:**

Create a file  `python_program.sbatch` to run `abc.py`.
```bash
#!/bin/bash

#SBATCH --ntasks=1              # Set number of tasks to run
#SBATCH --time=00:30:00         # Walltime format hh:mm:ss
#SBATCH -o job_%J.out           # Output file (%J: JobID)
#SBATCH -e job_%J.err           # Error file

# ---- Put all #SBATCH directives above this line! ---- #
# ---- Otherwise they will not be effective!       ---- #

# ---- Actual commands start here ---- #
# Load modules here (safety measure)
module purge
module load python

python abc.py
```

Submit the job:
```bash
sbatch python_program.sbatch
```

**A typical batch job script with GPU:**

```bash
#!/bin/bash

#SBATCH --nodes=1               
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=1
#SBATCH --time=5:00:00          # Walltime, expected time of job
                                # completion, SLURM can kill it 
                                # after this time
#SBATCH --mem=2GB               # RAM
#SBATCH --gres=gpu:1            # 1 GPU

#SBATCH --job-name=myTest       # Assigned Job Name
#SBATCH --mail-type=END                 # Email on completion
#SBATCH --mail-user=bob.smith@nyu.edu   # Email
#SBATCH --output=slurm_%j.out           # Output file
 
# Clean environment
module purge
module load cuda/11.3.1
module load anaconda3/2020.07 
eval "$(conda shell.bash hook)"

conda activate pytorch_env

cd $SCRATCH/Projects/DLProject
python resnet.py

```

```{tip}
If you want to select a specific GPU, you can specify that by `gpu:rtx8000:1` for Quadro RTX8000 and `gpu:v100:1` for Tesla V100.
```

```{admonition} Warning
:class: error

A job having a GPU resource is terminated if GPU usage is very low for 2 hours continuously.
```


## Interactive Sessions
Instead of submitting a job, you could get an interactive session from your terminal on compute nodes, where you can run your program on the shell directly.

```{admonition} Warning
:class: error

Only short interactive jobs should be used (e.g., experimenting with new hyper-parameters in your source code taking a short runtime on each execution).
```

To start an interactive session `srun` command is used.<br>
Request a session with 4 CPU cores:
```bash
srun -c 4  --pty /bin/bash
```

Expected output:

```bash
[wz22@login-0-1]$ srun -c 4  --pty /bin/bash
srun: job 775175 queued and waiting for resources
srun: job 775175 has been allocated resources
[wz22@compute-21-1 ~]$
```
Then you can run your applications on the terminal directly.

```{admonition} Warning
:class: error

In a real scenario, the system might be exhausted with no available resources to you. You need to wait in this circumstance.
```

With interactive session you can have the same arguments passed to `srun` as you pass into the job script with `sbatch`.<br>
Request a GPU session with 32 GB RAM and 10 CPU cores for 1 hour:
```bash
srun -c 10 --mem=32GB --gres=gpu:1 -t 1:00:00 --pty /bin/bash
```

To leave an interactive batch session, type `exit` at the command prompt.


## Checking Job Status

### Running or Pending Job

This command shows all your current jobs.

```bash
squeue -u <NetID>
```
Example output:

```bash
JOBID PARTITION     NAME     USER ST       TIME NODES NODELIST(REASON)
31408   ser_std  job1.sh     wz22  R       0:02     1 compute-21-4
```

It means the job with Job ID `31408`, has running status (ST: `R`) and its runtime is 2 minutes on `compute-21-4` cluster node.

For more verbose information, use scontrol show job.
```bash
scontrol show job <jobid>
```

### Completed Job
Once the job is finished, the job can’t be inspected by `squeue` or `scontrol show job`. At this point, you could inspect the job by `sacct`.
```bash
sacct -j <jobid>
```
The following commands give you extremely verbose information on a job.
```bash
sacct -j <jobid> -l
```

## Cancelling a Job

If you decide to end a job prematurely, use `scancel` commmand.

```bash
scancel <jobid>
```

````{admonition} Use with caution !
:class: caution

To cancel all jobs from your account. Run this on the HPC terminal.
```bash
scancel -u <NetID>
```
````

(resource-status)=
## HPC Resource Status

You can check current HPC compute resource status using the below dashboards:

- [Greene Cluster Load by Partition](https://graphs-out.hpc.nyu.edu/d/vA6e2Kgnk/greene-cluster-load-by-partitions-nyu-hpc-public?orgId=1&theme=light&refresh=30m&from=now-14h&to=now-5m&kiosk=tv)
- [Greene Resource Allocation & Utilization, Queue Status](https://graphs-out.hpc.nyu.edu/d/CEVdMFR7z/greene-cluster-nyu-hpc-public?orgId=1&theme=light&refresh=5m&from=now-14h&to=now-5m&kiosk=tv)

```{note}
You need to be on NYU Network or connected to {ref}`nyu-vpn`.
```