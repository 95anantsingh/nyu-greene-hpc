# Access
There are several ways to interact with the Greene HPC cluster. Similar to other Linux clusters, the most common method of connection is via a Command Line Interface (CLI).

```{image} assets/access.png
:alt: Architecture
:width: 90%
:align: center
```
<br>
<center><i>Generic diagram of the cluster architecture and outside connectivity</i></center>

(nyu-vpn)=
## NYU VPN

If you are connecting from a remote location that is not on the NYU network (your home for example), you have to use VPN to connect to NYU network.

1. Set up your computer to use the NYU VPN, a detailed guide to install on your local machine: 
    - [Windows](https://nyu.service-now.com/sp?id=kb_article&sysparm_article=KB0011177&sys_kb_id=6177d7031c811904bbcf4dc2835ec340&spa=1)
    - [Mac OS](https://nyu.service-now.com/sp?id=kb_article&sysparm_article=KB0011175&sys_kb_id=a6be768b1c8dd504bbcf4dc2835ec355&spa=1)
    - [Android](https://nyu.service-now.com/sp?id=kb_article&sysparm_article=KB0011176&sys_kb_id=da9ec34f1c011904bbcf4dc2835ec36c&spa=1)
    - [iPhone or iPad](https://nyu.service-now.com/sp?id=kb_article&sysparm_article=KB0011173&sys_kb_id=8fa0ee871cc9d504bbcf4dc2835ec345&spa=1)
    - [Linux](https://nyu.service-now.com/sp?sys_kb_id=8d0915bf13f526808073b5676144b063&id=kb_article_view&sysparm_rank=1&sysparm_tsqueryId=a8d3e3b1dbf0d990492a6d8d13961998) (No official support). 

    For more resources a generic VPN and MFA resource can be found [here](https://www.nyu.edu/life/information-technology/infrastructure/network-services/vpn.html).

1. Once you've created a VPN connection, you can proceed as if you were connected to the NYU net.


## SSH

SSH (Secure Shell) is a secure remote login and administrating protocol that enables users to login to the remote servers or systems securely to perform their tasks.
It allows users to login and works securely on the remote servers over an unsecured network. 

If you are connected to NYU network directly or through a VPN then you can simply `ssh` in your local terminal:

```bash
ssh <NetID>@greene.hpc.nyu.edu
```

Enter the same password which you use to login into your NYU account and you are good to go.

```{admonition} Warning
:class: error

Whenever you login, you land up on one of the login nodes and you should not run compute heavy jobs on login nodes directly, to avoid HPC account termination.
```

(saving-ssh-keys)=
### Saving SSH Keys

Instead of typing password every time you need to log in, you can also save ssh keys on your HPC account.

```{caution}
Only save keys from the computer you trust.
```

- Generate ssh keys on your local machine.

    ```bash
    ssh-keygen
    ```

    Follow the prompt, just hit Enter to go with default settings.
- Copy the key to HPC server.
  
    ```bash
    ssh-copy-id -i ~/.ssh/rsa_id.pub <NetID>@greene.hpc.nyu.edu
    ```

- Done, now you won't need to type your password again from your current machine.

## Web Interface

The HPC Web interface also known as OOD (built upon Open OnDemand ) is an interactive interface to remote computing resources which helps computational researchers and students efficiently utilize remote computing resources. The OOD Web Interface is one stop solution for all your HPC needs from accessing your files to submitting and viewing your jobs to running an interactive app.


Features Include:

- Easy file management - upload and download files, view HTML and pictures without downloading
- Command-line shell access without any SSH client locally installed
- Job management and monitoring
- Full Linux desktop experience without X11
- Interactive Apps such as JupyterHub and RStudio without the need for port forwarding

### Connection to OOD

To access interactive OOD interface visit: [https://ood.hpc.nyu.edu](https://ood.hpc.nyu.edu) (VPN Required), enter your NetID and password and click **Log in with your NYU account**.

```{image} assets/ood-login.png
:alt: OOD Login
:width: 85%
:align: center
```
<br>

### Access the Shell

Under the clusters menu you can select the Greene Shell Access option to access the Linux shell. No local SSH client is required.

```{image} assets/ood-shell-acess.gif
:alt: Shell Access
:width: 85%
:align: center
```

<br>

```{admonition} Common Error
:class: danger

A common issue that can occur is receiving an error that the Open OnDemand page cannot be reached. Sometimes this can indicate that the service is down, but often this is an issue with the the local browser cache. You can test this by opening a private browser window and seeing if OOD will load. If it does, try deleting the cache for *https://ood.hpc.nyu.edu* in your browser history to resolve this issue.
```

### Interactive Applications

GUI based applications are accessible without the need for port or X11 forwarding. Select the Interactive Apps menu, select the desired application, and submit the job based on required resources and options. 

```{image} assets/ood-interactive-apps.png
:alt: Interactive apps
:width: 85%
:align: center
```

<br>