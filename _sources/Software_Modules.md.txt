# Software Modules

HPC systems differ from desktop/laptop computers in how software is installed. In a desktop/laptop you install software in a pre-defined centralized location where all users can access it. But in a large system hosting a multitude of users it is not practical to have a single version of “python” installed. Some users may need access to the latest version, while others have hard dependencies on an earlier version. So in an HPC system software is installed as independent modules which users must explicitly load into their shell environment.

## Commands

When you login your shell environment is empty of all software modules and environment is set on a per shell instance. So if you open a new terminal on a login node the environment you may have set in another terminal will not be propagated. You can control and monitor the contents of your software modules environment with the following commands:

| Command                   | Description                                                               |
|---------------------------|---------------------------------------------------------------------------|
| `module avail`	        | check what software packages are available                                |
| `module load <name>`	    | load a module                                                             |
| `module unload <name>`	| unload a module                                                           |
| `module list`	            | check which modules are currently loaded in your environment              |
| `module purge`	        | remove all loaded modules from your environment                           |
| `module show <name>`	    | see exactly what effect loading the module will have with your environment|
| `module whatis <name>`	| Find out more about a software package                                    |
| `module help <name>`	    | A module file may include more detailed help for the software package     |

```{attention}
Never add `module load` into your .bashrc file.
```

```{tip}
Use `module purge` before loading a new module environment to run an application.
```

## Examples
<br>

**Get list of all available modules:**
```bash
module avail
```

**Get list of all the available versions of a module:**
```bash
module avail python

--- /share/apps/HPC/modules/ALL ---
python/2.7.11 python/3.5.1
```

**Load a specific module:**
```bash
# Only once after login or as required
module purge
module load python/3.5.1
```


```{note}
You can use the miniconda on HPC for hassle-free, independent Python environment. Access Anaconda setup guide [here](Anaconda.md).
```