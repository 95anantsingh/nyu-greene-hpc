# FAQs

Scheduling jobs in an efficient and fair manner can be a challenging task in a multi-user environment. Here are some frequently asked questions and recommendations.

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

## Why my jobs are queued?

To understand why your job is waiting in the queue you can run 
```bash
squeue  -j <JobID> -o "%.18i %.9P %.8j %.8u %.8T %.10M %.9l %.6D %R"
```

```{note}
You might also wanna look at {ref}`resource-limitations`.
```



## How to find more information on my jobs?

There are several commands to know more about pending, running and completed jobs. You should refer to {ref}`job-status` and {ref}`job-resource-stat`.


## How busy is the cluster?

Often your jobs are queued for a simple reason -- cluster is very busy and there aren't enough resources available. Please refer to the {ref}`resource-status` to see the number of jobs in the queue and other metrics. 

## Should I do Web scraping, Data mining from websites on HPC?
If you need to do web scraping, don't do that on Greene. It is not allowed due to security concerns and waste of CPU resources (most of the time a job would spend on downloading files, instead of using CPU). Please contact HPC Team, and they will advise on a better workflow for your project.

## How to check if I am wasting RAM and CPU?

You can check the resource utilization and efficiency by refering to {ref}`job-resource-stat` section.


<!-- SSH issues (potential)
Some people may experience connection warnings while connection to Greene, and connections being terminated too soon. 

This can be addressed by entering the following into ~/ssh/config 

# Increase alive interval
  host *
  ServerAliveInterval 60
  ForwardAgent yes
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null
  LogLevel ERROR -->
