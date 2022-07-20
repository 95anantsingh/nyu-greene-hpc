# Visual Studio Code

VS Code is a free editor from Microsoft, which is open source and support many useful extensions. 
It may be a good alternative to terminal based editors (like vim, nano and Emacs) for you.



```{image} assets/architecture-ssh.png
:alt: VSCode
:width: 80%
:align: center
```

<br>

**Benefits:**

- Work with files on cluster, almost as if they would be on your own laptop
- Great syntax highlighting, linting, etc. for many languages (using extensions)
- Extensive tools for debuging
- Extensions simplifying work with git
- Code, Images, Graphs produced on server, can be viewed directly withing VS Code
- When you are done editing - you can run sbatch job from terminal panel of VS Code
- You can set up port forwarding to extend the functionality

```{admonition} Warning
:class: error

Don't run computations in VS Code! <br>
Use VS Code only to edit scripts, use linting, etc.<br>
Only run programs using srun/sbatch
```


## Setup

### Download and Install
1. On your local machine install [Visual Studio Code](https://code.visualstudio.com/).
1. Install the [Remote Development extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack).

### Connecting to HPC cluster
1. Make sure you activated NYU VPN
1. Verify you can connect to the HPC SSH host by running the following command from a terminal.
    ```bash
    ssh <NetID>@greene.hpc.nyu.edu
    ```
1. In VS Code, select Remote-SSH: Connect to Host... from the Command Palette (`F1`, `Ctrl+Shift+P`) and use the same user@hostname as in previous step.
  <img src="https://code.visualstudio.com/assets/docs/remote/ssh/ssh-user@box.png"></img>



- VS Code Lock files shall NOT be saved in /home. 

In extensions tab of VS Code find extension "Remote - SSH"

press settings icon -> Extension settings

Do one of the following:

Don’t use lock files (uncheck "Remote.SSH: Use Flock")

Store lock file in /tmp (check Remote.SSH: Lockfiles In Tmp)

Inside VS Code

F1

remote-ssh: Connect to Host

New host (or - Configure SSH host)

ssh <user>@greene.hpc.nyu.edu

specify the path to the file, where the configuration of this connection will be saved

More details here: https://code.visualstudio.com/docs/remote/ssh

Login using ssh key

You can also specify ssh key.

Generate ssh key pair on terminal in Linux/Mac or cmd in Windows using keygen.

Note the path to ssh key files

Add content of public key (the one with .pub) to $HOME/.ssh/authorized_keys on cluster

Add path to ssh key on your machine to VS Code config file (you chose path to this file above, when setting up remote connection)

Host greene.hpc.nyu.edu
  HostName greene.hpc.nyu.edu
  User <netid>
  ForwardAgent yes
  IdentityFile <path to ssh key>
More settings
VS Code has many more settings and options. Explore!

Additional notes
High CPU load on login node from VS Code

You may notice (please check with 'top') that your VS Code connection causes 'node' process running from your user to use a lot of CPU resources. One of the reason leading to that - large number of files within your home directory. Try to remove conda and pip environments from the home directory and check if this will resolve the issue.

Python

Recommended extensions: Python, Anaconda Extension Pack

When those are installed you can switch python env or conda env by clicking on the left bottom line specifying python.

Support for python scripts and Jupyter Notebooks

Syntax highlighting: extension MagicPython

## Workspace Management


## Recommended Extensions

 (recommended: Git Graphs)