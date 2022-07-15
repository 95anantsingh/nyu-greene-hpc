# Greene Overview

NYU High Performance Computing (NYU HPC) provides access to state of the art supercomputer hardware and cloud services to eligible faculty and students across all of NYU.

The cluster is accessed through login nodes which are nodes or servers dedicated to provide HPC users access to compute nodes. Compute nodes are more powerful and optimized to run computations as per the jobs submitted by HPC users. 

NYU Greene runs on 
<!-- 
![](assets/access.png) -->


## Specifications

### Hardware
- The Total number of available nodes is 672
    1. 6 login nodes
    1. 670 compute nodes
        - 524 Standard memory (180 GB)
        - 40 Medium memory (360 GB)
        - 4 Large memory (3,014 GB)
        - 73 RTX8000 GPU nodes
        - 20 V100 GPU nodes
        - 9 A100 GPU nodes
    1. 6 administrative nodes

- Total number of CPU cores is 31,584
- Total number of GPU cards is 368 
    1. 292 RTX8000 (48 GB)
    1. 76 V100 (32 GB)
- Total primary memory is 163 TB
- Total secondary memory is 9.5 PB

### Software

- **Operating system** - RedHat Enterprise Linux 8.4
- **Workload manager** - SLRUM

```{admonition} Useful Links
:class: hint

1. Linux
    - [https://www.geeksforgeeks.org/essential-linuxunix-commands](https://www.geeksforgeeks.org/essential-linuxunix-commands)
    - [https://www.tutorialspoint.com/unix/index.htm](https://www.tutorialspoint.com/unix/index.htm)
    - [http://software-carpentry.org/lessons/](http://software-carpentry.org/lessons/)
    - [https://www.edx.org/course/introduction-linux-linuxfoundationx-lfs101x-0](https://www.edx.org/course/introduction-linux-linuxfoundationx-lfs101x-0)
<br>
1. SLRUM 
    - [https://slurm.schedmd.com/quickstart.html](https://slurm.schedmd.com/quickstart.html)
    - [https://slurm.schedmd.com/tutorials.html](https://slurm.schedmd.com/tutorials.html)
    - [https://www.youtube.com/watch?v=U42qlYkzP9k&feature=player_embedded](https://www.youtube.com/watch?v=U42qlYkzP9k&feature=player_embedded)


```

### Network

- **Infiniband Network** : for MPI and file system access, by Melanox.
- **Management Network** : Ethernet 25Gbit  used by admins for node provisioning
- **Out-of-band Network** : Ethernet 1Gbit used by admins
- **External Network** : All cluster 'edge' nodes, such as login nodes, admin nodes, and Data Transfer Nodes, are connected to the NYU High-Speed Research Network (HSRN).


## Typical Workflow

1. Log on to HPC login nodes.
1. Submit jobs on login nodes.
1. Your jobs will queue for execution on 
1. Once done, examine the output.


## Storage Management

The NYU HPC clusters are served by a General Parallel File System (GPFS) cluster. 

The NYU HPC team supports data storage, transfer, and archival needs on the HPC clusters, as well as collaborative research services like the Research Project Space (RPS).

| Space<br> | Environment <br> Variable | Space Purpose<br> | Backed Up / <br> Flushed | Quota <br> Disk Space / # of Files |
|-------|----------------------|---------------|---------------------|-------------------------------|
| `/home` | $HOME	               | Personal user home space that is best for small files	| YES / NO | 50 GB / 30 K |
| `/scratch` | $SCRATCH	| Best for large files | NO / Files not accessed<br> for 60 days | 5 TB / 1 M |
| `/archive` | $ARCHIVE	| Long-term storage	| YES / NO | 2 TB / 20 K |
| HPC Research <br> Project Space| NA | Shared disk space for research projects | YES / NO | Payment based <br>TB-year/inodes-year |