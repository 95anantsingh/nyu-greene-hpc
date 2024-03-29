# Jupyter Notebooks

Open OnDemand is a tool that allows users to launch Graphical User Interfaces (GUIs) based applications are accessible without modifying your HPC environment. OOD allows users to launch Jupyter Lab sessions from [https://ood.hpc.nyu.edu](https://ood.hpc.nyu.edu).

```{important}
Please setup `conda` before proceeding further. For more info {ref}`conda-setup`.
```

## One time Setup

1. Logon into HPC via a terminal.

1. Create a directory for the environment
    ```bash
    mkdir /scratch/$USER/singularity
    cd /scratch/$USER/singularity
    ```

1. Prepare Overlay, for 400K files andupto 10GB of storage space.
    ```bash
    cp -rp /scratch/work/public/overlay-fs-ext3/overlay-10GB-400K.ext3.gz .
    gunzip overlay-10GB-400K.ext3.gz
    mv overlay-10GB-400K.ext3 partition.ext3
    ```
    ````{note}
    You can browse available overlay images to see available options:
    ```bash
    ls /scratch/work/public/overlay-fs-ext3
    ```
    ````

1. Launch Singularity environment for installation
    ```bash
    singularity exec --overlay /scratch/$USER/singularity/partition.ext3:rw /scratch/work/public/singularity/cuda11.6.124-cudnn8.4.0.27-devel-ubuntu20.04.4.sif /bin/bash
    ```
    ````{note}
    You can browse available OS images to see available options:
    ```bash
    ls /scratch/work/public/singularity/
    ``` 
    ````
    ```{important}
    Be sure that you have the Singularity prompt (Singularity>) before the next step.
    ```


1. Install Miniconda to overlay file
    ```bash
    wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
    sh Miniconda3-latest-Linux-x86_64.sh -b -p /ext3/miniconda3
    ```
    Remove downloded package
    ```bash
    rm Miniconda3-latest-Linux-x86_64.sh
    ```

1. Create an invoking wrapper script
    ```bash
    touch /ext3/env.sh
    echo '#!/bin/bash' >> /ext3/env.sh
    echo 'source /ext3/miniconda3/etc/profile.d/conda.sh' >> /ext3/env.sh
    echo 'export PATH=/ext3/miniconda3/bin:$PATH'         >> /ext3/env.sh
    echo 'export PYTHONPATH=/ext3/miniconda3/bin:$PATH'   >> /ext3/env.sh
    ```
    Source Anaconda     
    ```bash
    source /ext3/env.sh
    ```

1. Confirm Installation<br>
    Check conda location
    ```bash
    which conda
    ```
    Expected Output:
    ```bash
    /ext3/miniconda3/bin/conda
    ```
    Check python location
    ```bash
    which python
    ```
    Expected Output:
    ```bash
    /ext3/miniconda3/bin/python
    ```

1. Update, clean base conda environment and install necessary packages
    ```bash
    conda update -n base conda -y
    conda clean --all -y
    conda install pip -y
    conda install ipykernel -y
    ```

    ```{important}
    `ipykernel` is required to run Open OnDemand Jupyter Notebooks, please install this package to your environment if haven't already.
    ```

1. Exit Singularity image
    ```bash
    exit
    ```

## Add Jupyter Kernel

You need to add one kernel per environment that you want to use with Jupyter Notebook.

1. Load anaconda module
    ```bash
    module purge
    module load anaconda3/2020.07
    eval "$(conda shell.bash hook)"
    ```

1. Create a conda environment outside of Singularity (Skip this step if you have already an environment other than base)
    ```bash
    conda create -n <env_name> python=3.8
    ```

1. Install required packages to your environment
    ```bash
    conda activate <env_name>
    conda install ipykernel ipywidgets -y
    ```

1. Configure iPython kernels
    ```bash
    mkdir -p ~/.local/share/jupyter/kernels
    cd ~/.local/share/jupyter/kernels
    cp -R /share/apps/mypy/src/kernel_template ./<env_name>
    cd ./<env_name>
    ```

1. Edit `kernel.json`

    ```bash
    nano kernel.json
    ```

    Edit the file so that it should look like:
    ```json
    {
        "argv":[
                "/home/<Your NetID>/.local/share/jupyter/kernels/<env_name>/python",
                "-m",
                "ipykernel_launcher",
                "-f",
                "{connection_file}"],
        "display_name": "<kernel_name>",
        "language": "python"
    }
    ```
    ```{important}
    Update `<Your NetID>` to your own NetID, `<env_name>` to the name of your conda environment and `<kernel_name>` to a name for this kernel.

    ```
    
    For Example:
    ```json
    {
        "argv":[
                "/home/abc123/.local/share/jupyter/kernels/nlp/python",
                "-m",
                "ipykernel_launcher",
                "-f",
                "{connection_file}"],
        "display_name": "NLP Env",
        "language": "python"
    }
    ```

    Save the file by pressing `Ctrl+X`, then hit `Y` and hit `Enter` to confirm.

1. Edit `python` file
    ```bash
    nano python
    ```

    Edit the file so that it should look like:
    ```bash
    #!/bin/bash

    args=''
    for i in "$@"; do 
    i="${i//\\/\\\\}"
    args="$args \"${i//\"/\\\"}\""
    done

    unset XDG_RUNTIME_DIR
    if [ "$SLURM_JOBTMP" != "" ]; then
    export XDG_RUNTIME_DIR=$SLURM_JOBTMP
    fi

    if [[ "$(hostname -s)" =~ ^g[r,v] ]]; then nv="--nv"; fi

    cmd=$(basename $0)


    singularity exec $nv --overlay /scratch/$USER/singularity/partition.ext3:ro \
                /scratch/work/public/singularity/cuda11.6.124-cudnn8.4.0.27-devel-ubuntu20.04.4.sif \
                /bin/bash -c "source /ext3/env.sh;conda activate <env_name>; $cmd $args"

    ```
    ```{important}
    Update `<env_name>` to the environment you are doing the setup without the "<>" symbols.
    ```
    
    Save the file by pressing `Ctrl+X`, then hit `Y` and hit `Enter` to confirm.

    ```{caution}
    If you used a different overlay or sif file, change those lines in the command above to the files you used.
    ```

## Launch Jupyter Notebook

1. Logon to [https://ood.hpc.nyu.edu](https://ood.hpc.nyu.edu) (VPN Required)

```{image} assets/ood-login.png
:alt: OOD Login
:width: 80%
:align: center
```
<br>

2. Launch Jupyter Notebook under **Interactive Apps**

```{image} assets/ood-jupyter.png
:alt: OOD Jupyter
:width: 90%
:align: center
```
<br>

3. Request resources. Select Jupyter Lab option for launching Jupyter Lab Session (Recommended).

```{image} assets/ood-jnb-session.png
:alt: OOD Jupyter Session
:width: 80%
:align: center
```
<br>

4. Select Kernel
    <br>
    Once configured and launched, kernels can be selected in the "New" dropdown or within the notebook under the kernel menu. Please note that your notebook view may look slightly different depending on available directories and environments, as well as if you choose the lab or traditional notebook view.

```{image} assets/ood-kernel2.png
:alt: OOD Jupyter Kernel
:width: 90%
:align: center
```
<br>
<br>