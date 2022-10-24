# Storage

Users may have different storage requirements and HPC provides 4 storage systems that users can take advantage of.

```{caution}
Backing up is a user’s own responsibility. E.g., if a user deleted something accidentally, it can not be recovered, unfortunately although you could try looking in Trash Bin.
```

## File Systems

The NYU HPC clusters are served by a General Parallel File System (GPFS) cluster. 

The NYU HPC team supports data storage, transfer, and archival needs on the HPC clusters, as well as collaborative research services like the Research Project Space (RPS).

| Space<br> | Environment <br> Variable | Space Purpose<br> | Backed Up / <br> Flushed | Quota <br> Disk Space / # of Files |
|-------|----------------------|---------------|---------------------|-------------------------------|
| `/home` | $HOME	               | Personal user home space<br> that is best for small files	| YES / NO | 50 GB / 30 K |
| `/scratch` | $SCRATCH	| Best for large files | NO / Files not<br> accessed for 60 days | 5 TB / 1 M |
| `/archive` | $ARCHIVE	| Long-term storage	| YES / NO | 2 TB / 20 K |
| HPC Research <br> Project Space| NA | Shared disk space for<br>research projects | YES / NO | Payment based <br>TB-year/inodes-year |

<!-- 
| | $HOME |$SCRATCH	|$WORK	|$ARCHIVE|			
|--|------|--------|---------|-------|
|Use for storing | source code / executables |data	| anything	|anything|
|Accessible From |	login / compute|	login / compute|	login|	login|
|Use to Run Jobs |	No|	Yes|	No|	No|
|Retention Time (Days) | 	No Limit|	90|	120|	No Limit|
|Mountable| 	No|	No|	Yes|	No|
|Default Quota (star)|	20GB, 150K Files|	5TB, 500K Files|	5TB, 500K Files|	5TB, 125K Files| -->

```{caution}
Running jobs from `/home` is not recommended. `/home` SSDs are not designed for this purpose, it will kill the SSDs quickly.
```

```{admonition} Warning
:class: error

Pay attention to data retention / flush cycles.
```

In short, you should

- Put all your data to `/scratch/<NetID>` and run your jobs from there.
- Only a small persistent fraction to `/home/<NetID>` (e.g., source code, executables).
- For long-term storage, archive them to   `/archive/<NetID>`.

Users should clean up their storage regularily.

````{tip}
You can access any file system using environment variable or its absolute path, for example both statements are equivalent:

```{code-block} bash
cd $HOME
cd /home/$USER
```
````


## Quota Utilization

Users can check their storage utilization by running:

```{code-block} bash
myquota
```

Example output:

```{code-block} bash

                    DISK SPACE                # FILES (1000's)
filesystem       size      quota            number      quota
            --------------------------   --------------------------
/home         131KB     20GB   ( 0%)           0        150 ( 0%)
/scratch      220GB     5242GB ( 4%)           4        500 ( 1%)
```

## Data Transfer

The recommended way to transfer data to/from HPC systems is via OpenOnDemand HPC web interface.

- Connect to VPN (If not on NYU network).
- Logon to OOD website [https://ood.hpc.nyu.edu](https://ood.hpc.nyu.edu).
- Go to Files tab on top.
- From there you can transfer files as you wish.

```{image} assets/ood-files.png
:alt: OOD Files
:width: 90%
:align: left
```

<br>

```{tip}
If you use **VS Code**, then you can directly use **Explorer** tab of VS Code to transfer files by just copy pasting to/from Explorer tab.
```


(storage-status)=
## HPC Storage Status

On the Dashboard you can find current utilization and allocation data for the following file system mounts
- GPFS file system: /home, /scratch, /archive
- VAST file system: /vast
- HDFS file system of Hadoop cluster Peel

Dashboard: [Storage NYU HPC](https://graphs-out.hpc.nyu.edu/d/0_16dHc7z/storage-nyu-hpc-public?orgId=1&theme=light&refresh=5m&from=now-14h&to=now-5m&kiosk=tv)

```{note}
You need to be on NYU Network or connected to {ref}`nyu-vpn`.
```
