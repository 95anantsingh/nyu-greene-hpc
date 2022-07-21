# Best Practices

Scheduling jobs in an efficient and fair manner can be a challenging task in a multi-user environment. Here are some recommendations.

## What resources to ask for (and how much)?

When asking for compute resources in the batch script, never ask for more than you need. There are two important reasons listed below. Imagine you need only 2 CPUs but request 10.

- You will wait longer for job to start: It is easier for scheduler to find 2 CPUs than 10
- You will make other users to not be able to run jobs: 8 cores will be sitting idle and no other users will be able to use them.

The same argument applies to other types of resources: RAM, GPUs (and potentially other 'TRES' in SLURM terminology).
```{important}
Most of the times you only need 1 core, do not assume and check if your code actually need more than one core. <br>
Only request what you need.
```
Very often, even for parallel codes, it makes sense to request fewer CPU cores. Main example of this is -- your parallel job doesn't scale well. For example, imagine that using 12 cores instead of 4, you can reduce execution time from 1 hour (on 4 cores) to 45 minutes (on 12 cores). Yes, 45 minutes is faster, but you are using 3 times as many resources. Additionally, it may happen that your job with 4 cores will only wait in the queue for 5 minutes, but the 12-core job would need to wait for 20 minutes. This completely  offsets all the gains from using 8 more cores. In the end you are to judge, what is really needed, but you should always make this type of considerations when creating a job script. 

```{important}
It is recommend that you don't specify a specific GPU unless needed.
```
  
```{admonition} Warning
:class: error
Do NOT run CPU heavy jobs on login nodes <br>
Login nodes designed use: <br>
    - Logging In
    - Running low-compute operations like copying files, installing packages, etc

```


## Resource Usage Statistics

### Completed Job
A useful command that allows you to better understand how resources were utilized by completed jobs is `seff`:

```bash
seff <job-ID>
```
Example Output:
```log
Job ID: 8932105
Cluster: greene
User/Group: NetID/GROUPID
State: COMPLETED (exit code 0)
Cores: 1
CPU Utilized: 02:22:45
CPU Efficiency: 99.99% of 02:22:46 core-walltime
Job Wall-clock time: 02:22:46
Memory Utilized: 2.18 GB
Memory Efficiency: 21.80% of 10.00 GB
```

This example shows statistics on a completed job, that was ran with a request of 1 cpu core and 10Gb of RAM. While CPU utilization was 100%, RAM utilization was very poor -- only 2.2GB out of requested 10GB was used. This job's batch script should definitely be adjusted to something like #SBATCH --mem=2250MB

### Running job

From login node, run:
```bash
top -u <NetID>
```

Take a look how fully you use CPUs and how much RAM your jobs are using.

To exit hit `Ctrl+C`
<br>
For a GPU job also run:
```bash
nvidia-smi
```

Take a look how much GPU processing power your job is using.

To exit hit `Ctrl+C`


## Why my jobs are queued?
To understand why your job is waiting in the queue you can run 

squeue  -j <JobID> -o "%.18i %.9P %.8j %.8u %.8T %.10M %.9l %.6D %R"
Last column of the output would indicate a reason. You can find out more about squeue output format from man squeue. 

The column "NODELIST(REASON)" in the end is job status due to the reason(s), which can be :

Priority: higher priority jobs exist

Resources: waiting for resources to become available

BeginTime: start time not reached yet

Dependency: wait for a depended job to finish

QOSMaxCpuPerUserLimit: number of CPU cores limit is reached

JobArrayTaskLimit: limit of tasks in the JobArray is reached

 For a more complete list of possible values of REASON, please refer to man squeue under the section JOB REASON CODES.

SLURM Limits
Within SLURM there are multiple limits defined on different levels and applied to different objects. Some of the important limits are listed here. 

There is a limit of 2000 jobs per user. 

Job lifetime is limited to 7 days (168 hours), but you can request an extension by emailing HPC team (hpc@nyu.edu).

One important limitation regulates how many CPU cores are available per user in different partitions. You can get this by running:

####sacctmgr list qos format=name,maxwall,maxtresperuser%40,flags%40 where name=interact,cpu48,cpu168,gpu48,gpu168,gpuamd,cds,cpuplus,gpuplus

      Name     MaxWall                                MaxTRESPU                                    Flags
---------- ----------- ---------------------------------------- ----------------------------------------
     cpu48  2-00:00:00                       cpu=3000,mem=6000G
    cpu168  7-00:00:00                       cpu=1000,mem=2000G
     gpu48  2-00:00:00                              gres/gpu=24
    gpu168  7-00:00:00                               gres/gpu=4
  interact    04:00:00                        cpu=48,gres/gpu=4
   gpuplus                                          gres/gpu=72
       cds                                          gres/gpu=32
   cpuplus                                             cpu=9000
    gpuamd                                         gres/gpu=160
From this you can see that in the "short queue" (under 48 hours, or 2 days) each user is allowed to utilize up to 3000 cores. For jobs that are running in the "long queue" (under 168 hours, or 7 days) you can use up to 1000 cores. Basic idea behind this -- users can run more short jobs, and fewer long jobs. The same logic applies to GPU resources. 

Note, that these limits are frequently updated by the HPC team, based on the cluster usage patterns. Due to this, the numbers below are not exact and should only be used as general guidelines. 

Another important limits to be aware of are the limits on how many CPU cores can be used together with GPU(s). Here are some of these limits:

| # gpus | max_cpus | max_memory |
         gpu type = "V100"      
|--------+----------+------------|
|      1 |       20 |        200 |
|      2 |       24 |        300 |
|      3 |       44 |        350 |
|      4 |       48 |        369 |
         gpu type = "rtx8000"            
|--------+----------+------------|
|      1 |       20 |        200 |
|      2 |       24 |        300 |
|      3 |       44 |        350 |
|      4 |       48 |        369 |


From this table you can for example see, that a job asking for 8 V100 GPUs will not be queued. Another example is that requests for 2 V100s and 48 cores will also not be granted. 

How to find more information on my jobs?
Some other useful SLURM commands that can help to get information about running and pending jobs are
<!-- 
# detailed information for a job:
scontrol show jobid -dd <jobid>

# show status of a currently running job
# (see 'man sstat' for other available JOB STATUS FIELDS)
sstat --format=TresUsageInMax%80,TresUsageInMaxNode%80 -j <JobID> --allsteps

# get stats for completed jobs 

# (see 'man sacct' for other JOB ACCOUNTING FIELDS)
sacct -j <jobid> --format=JobID,JobName,MaxRSS,Elapsed


# the same information for all jobs of a user:
sacct -u <username> --format=JobID,JobName,MaxRSS,Elapsed 


How busy is the cluster?
Please refer to the Systems Status page to see the number of jobs in the queue and other metrics. Often your jobs are queued for a simple reason -- cluster is very busy and there aren't enough resources available.

Best way to submit large number of similar jobs
The correct way to submit such jobs is to use the array job functionality of SLURM. This reduces load on the scheduler system.

Don't make your own loops to do this kind of work.

You can find a detailed description on how to submit such jobs here.

Web scraping, Data mining from websites
If you need to do web scraping, don't do that on Greene. It is not allowed due to security concerns and waste of CPU resources (most of the time a job would spend on downloading files, instead of using CPU). Please contact us, and we will advise on a better workflow for your project.

Error handling
We recommend using #!/bin/bash -e instead of plain #!/bin/bash, so that the failure of any command within the script will cause your job to stop immediately rather than attempting to continue on with an unexpected environment or erroneous intermediate data. It also ensures that your failed jobs show a status of FAILED in sacct output.

When a batch job is finished it produces an exit code (among other useful data). To view the error code of the job you can use: 

sacct -b -j <JobID>
When reaching out to the HPC team asking for help with failing jobs, it is useful to find an exit code from the job at question. 



SSH issues (potential)
Some people may experience connection warnings while connection to Greene, and connections being terminated too soon. 

This can be addressed by entering the following into ~/ssh/config 

# Increase alive interval
  host *
  ServerAliveInterval 60
  ForwardAgent yes
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null
  LogLevel ERROR -->
