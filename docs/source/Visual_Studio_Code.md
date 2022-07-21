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

- Work with files on cluster, almost as if they would be on your own laptop. Cut, copy, paste from Explorer tab cross platform to/from HPC cluster.
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

1. VS Code Lock files shall not be saved in `/home` and you need to apply changes to VS Code settings. <br>
Go to **File > Preferences > Settings** , Search for `remote.SSH.useFlock` and *Enable* **Remote.SSH: Use Flock** setting.
1. In VS Code, select Remote-SSH: Connect to Host... from the Command Palette (`F1`, `Ctrl+Shift+P`) and use the same `<NetID>@greene.hpc.nyu.edu` as in previous step to login.

<center><img src="_images/ssh-host.png" style="max-width:65%;" alt></img></center>
<br>

5. Select **Linux** as the type of server if prompted.
1. After a moment, VS Code will connect to the SSH server and set itself up. VS Code will keep you up-to-date using a progress notification and you can see a detailed log in the Remote - SSH output channel.

1. You can then open any folder or workspace on the remote machine using **File > Open... or File > Open Folder...** just as you would locally!


```{tip}
You can {ref}`Save SSH Keys<saving-ssh-keys>` on HPC cluster so that you don't have to enter your password again.
```


```{caution}
You may notice (please check with 'top') that your VS Code connection causes 'node' process running from your user to use a lot of CPU resources. One of the reason leading to that - large number of files within your home directory. Try to remove conda and pip environments from the home directory and check if this will resolve the issue.
```


## Workspace Management

A Visual Studio Code "Workspace" is the collection of one or more folders that are opened in a VS Code window (instance). You can manage your data and code at one place.

This allows users to keep Data and Code seperate which help in maintaining Storage Quota in check. This also helps to keep different settings for different folders in the project.

- You can add folders to your workspace from - **File > Add Folder to Workspace...**
- This will generate a workspace file and you can open it instead of any folder to load all the folders by default.


## Recommended Extensions

- [Git Graphs](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)
- [MagicPython](https://marketplace.visualstudio.com/items?itemName=magicstack.MagicPython)