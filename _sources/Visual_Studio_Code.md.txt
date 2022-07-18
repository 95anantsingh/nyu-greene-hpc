# Visual Studio Code

## Setup

## Workspace Management


VS Code is a free editor from Microsoft, which is open source and support many useful extensions. 

It may be a good alternative to terminal based editors (like vim, nano and Emacs) for some of our users.

Benefits

Work with files on cluster, almost as if they would be on your own laptop

Great syntax highlighting, linting, etc. for many languages (using extensions). Tools for debuging

Extensions simplifying work with git (recommended: Git Graphs)

Graphs produced on server, can be viewed directly withing VS Code

When you are done editing - you can run sbatch job from terminal panel of VS Code

You can set up port forwarding

And more

IMPORTANT: don't run computations in VS Code!
Use VS Code only to edit scripts, use linting, etc.

Only run calculations using srun/sbatch

Download and install
Download and install it on your local machine: download

Setting up remote connection to HPC cluster
Make sure you activated NYU VPN

VS Code Lock files shall NOT be saved in /home. Ways to achieve that

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