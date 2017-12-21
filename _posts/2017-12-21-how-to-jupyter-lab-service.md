---
published: true
date: '2017-12-21 10:00:00'
title: 'How to: Jupyter Lab User Service'
permalink: /2017/jupyter-lab-service
author: bradford
layout: post
categories:
  - Technology
  - Scripts
  - How To
tags:
  - Jupyter Lab
  - Python
  - Anaconda
  - Node.js
comments: true
---
I am doing some data science at the moment. I have been working heavily on a project in Python that collects data. I'm at the stage now where I am working with the collected data. I initially started using Matlab for this purpose, but I spent so much time learning the Matlab syntax, that I decided to switch to Python for my data science needs. 

I started with Spyder, which is pretty good, albeit rough around the edges coming from PyCharm. I learned about [Jupyter](https://jupyter.org/) and the new [Jupyter Lab](https://github.com/jupyterlab/) project, which, while in active development, looks like it might be a good fit for my data science needs.

In this how-to, I will guide you through creating a user-space Anaconda environment, install Jupyter Lab, and create a `systemctl` service that will automatically run Jupyter Lab at startup. I did everything in an Ubuntu 17.04 Server (headless) virtual machine running in VMWare Workstation 14 on Windows 10, although this should work for any Linux distribution with `systemctl`.

![Jupyter Logo](/assets/images/posts/2017/jupyter_logo.svg)

## Step 1: Set Up Your Environment

### Install Pyenv
[`Pyenv`](https://github.com/pyenv/pyenv) is a fantastic script that will manage your python environment so you don't have to worry about changing the system default python environment.

> #### Pyenv Prerequisites
> You need a system with [certain build packages](https://github.com/pyenv/pyenv/wiki/Common-build-problems) installed already - so if you don't have root access, talk to your sysadmin to get these installed. Chances are they already are.
>
>    ```bash
>    $ sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
>    libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
>    xz-utils tk-dev
>    ```

With these installed, you're ready to install `pyenv`.

The simplest way to install pyenv is through [pyenv installer](https://github.com/pyenv/pyenv-installer), with the following commands:

```bash
$ curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash
```

The installer will spit out a blob of code it thinks you should put in your ~/.bash_profile file, something like this:

```bash
export PATH="~/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

Update pyenv with this command: 

```bash
$ pyenv update
```

> If you get a `command not found` error, you may need to reload your ~/.bash_profile so your current shell session knows where the pyenv command is:
> 
> ```bash
> $ source ~/.bash_profile
> ```

You now have a working `pyenv` environment.

### Install Anaconda
Pyenv is great because you can install many versions of Python, or Python distributions such as Anaconda or Miniconda.

To see a list of the available distributions, type

```bash
$ pyenv install -l
```

Jupyter is part of Anaconda, so we'll install the current (as of this guide) release of Anaconda:

```bash
$ pyenv install anaconda3-5.0.1
```

This step will take some time, since Anaconda is a hefty distribution.

Once Anaconda is installed, activate it with the `activate` subcommand. Pyenv supports tab completion which makes many commands easier:

```bash
$ pyenv activate ana<tab key> 
$ pyenv activate anaconda3-5.0.1
(anaconda3-5.0.1) $
```

You  may also deactivate the current `pyenv` with the command 

```bash
(anaconda3-5.0.1) $ pyenv deactivate
$
```

### Install Node.js
We need Node.js for the Jupyter Lab plugins, and we can install that to our user profile as well with [Node Version Manager (NVM)](https://github.com/creationix/nvm).

To install `nvm`, use the following command:

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
```

`nvm` automatically adds the proper environmental variables to your ~/.bashrc file, but let's move it to your ~/.bash_profile. Edit ~/.bashrc with your editor of choice, and look for the following lines (close to the end):

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Cut them from ~/.bashrc and paste them into ~/.bash_profile

You won't have access to `nvm` until you either log out or reload ~/.bash_profle with

```bash
$ source ~/.bash_profile
```

Now that we have `nvm` installed, let's install `node.js`. To see a list of available `node.js` distributions available, type

```bash
$ nvm ls-remote
```

To install the most current LTS release of `node.js`, type

```bash
$ nvm install --lts
```

Now we have an environment ready for Jupyter Lab.

## Step 2: Install & Configure Jupyter Lab

### Install Jupyter Lab
Installing Jupyter Lab is as simple as a single `conda` command from the Anaconda environment:

```bash
$ pyenv activate anaconda3-5.0.1
(anaconda3-5.0.1) $ conda install -c conda-forge jupyterlab
```

The script will ask you to upgrade (or downgrade) certain modules to accomodate `jupyterlab`. 

Once it is installed, test that it works with the command

```bash
(anaconda3-5.0.1) $ jupyter lab
...<logs>...
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8888/?token=1ba6d4c77e6b6028365e2467f5134ce128f5d3a649aa2a26
```

If your command was successful, you'll see output like that above, with a link and token. If you are running `jupyterlab` on a different machine, you won't be able to access it since it is not listening to external requests (only `localhost` by default). 

### Configure Jupyter Lab
Configuring `jupyterlab` isn't very straightforward since the documentation is pretty sparse. Much of the documentation for jupyter `notebook` applies to `jupyterlab`, however.

#### Listen on an external IP address
The `--ip` argument specifies which IP address to listen for connection requests on, so determine your ip address with `ip address` command

```
--ip=<your ip address>
```

#### Enable SSL
If you want to enable SSL, follow these steps as well, pulled from the [Jupyter Notebook docs](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html).

Make a directory to store your keys and self-signed cert. I created mine in `~/.jupyter`, which is the default `jupyterlab` config directory (use the command `(anaconda3-5.0.1) $ jupyter --config-dir` to see yours). Once you have a directory for your keys and cert, use the following command to generate them:

```bash
(anaconda3-5.0.1) $ mkdir -p ~/.jupyter/keys
(anaconda3-5.0.1) $ cd ~/.jupyter/keys
(anaconda3-5.0.1) $ openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mykey.key -out mycert.pem
```

Now, we use the command line parameters `--keyfile` and `--certfile` to use when setting up our `jupyterlab` server:

```
--keyfile=~/.jupyter/keys/mykey.key
--certfile=~/.jupyter/keys/mycert.pem
```

#### Test Parameters
Now we can test these parameters to make sure they work, and we can access Jupyter Lab from an external browser:

```bash
(anaconda3-5.0.1) $ jupyter lab --ip=<your ip address> --keyfile=~/.jupyter/keys/mykey.key --certfile=~/.jupyter/keys/mycert.pem
...<logs>...
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        https://<your ip address>:8888/?token=35b5492a621e16fdcc97c0a9d7e3c4241416737ca8e7f0fd
```

You should now be able to access that URL and access Jupyter Lab.

> You may have to click through self-signed cert warnings in your browser. The Jupyter Notebook docs have [instructions on how to get trusted SSL certs](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html#using-let-s-encrypt) as well.

![Jupyter Lab](/assets/images/posts/2017/Jupyter_Lab.png)

## Step 3: Running Jupyter Lab as a User Service
There are certain advantages to creating your environment using `pyenv` and `nvm`, however running services as a user is a little tricky. 

The first step is to create and modify a Jupyter Notebook config file. I assume that since Jupyter Lab is beta, it will eventually have its own config file, but for now, it uses the Notebook config. 

### `jupyter_notebook_config.py`

Initialize a Jupyter Notebook config file with the command 

```bash
(anaconda3-5.0.1) $ jupyter lab --generate-config
```

You now have a file called `jupyter_notebook_config.py` in your `jupyter` config directory (most likely `~/.jupyter`). Edit that file, and let's place some of your command line arguments there instead. Modify the following lines

```bash
#c.NotebookApp.keyfile = ''
#c.NotebookApp.certfile = ''
```

to 

```bash
c.NotebookApp.keyfile = '/home/<YOUR USER>/.jupyter/keys/mykey.key'
c.NotebookApp.certfile = '/home/<YOUR USER>/.jupyter/keys/mycert.pem'
```

> **Note:** You must use absolute paths here, since a home path with `~` doesn't work in our script below.

You may also specify your ip address in this config file as well (by uncommenting and modifying `#c.NotebookApp.ip = 'localhost'`), however if your environment has dynamic ip addresses, you can dynamically determine your ip address in the startup script below.

#### Enable Password Login
If you want to log in using a password, use the following command to generate a password in your config file:

```bash
(anaconda3-5.0.1) $ jupyter notebook password
Enter password:  ****
Verify password: ****
[NotebookPasswordApp] Wrote hashed password to ~/.jupyter/jupyter_notebook_config.json
```

You will now be prompted for a password instead of required to use the generated token. 

### `jupyterlab.sh`

Create a file called `jupyterlab.sh` in your home directory (or anywhere else), and open it up in your favorite text editor. 

```bash
#!/bin/bash
eval "$(pyenv init -)"
pyenv activate anaconda3-5.0.1
cd <your jupyterlab working directory>
IP=`ip -4 route get 8.8.8.8 | awk {'print $7'} | tr -d '\n'`
jupyter lab --no-browser --ip=$IP
```

Modify the 4th line above (`cd`) to change directory to your data working directory. 

Line 5 will start `jupyterlab` on the current ip address that is routable to the internet. You can change this or remove this and statically set the ip addres as well.

Save the file and `chmod` it to allow execution:

```bash
(anaconda3-5.0.1) $ chmod +x jupyterlab.sh 
```

You may test this script by running the script (in or out of the anaconda environment):

```bash
$ ./jupyterlab.sh
```

### Create Jupyter Lab startup script
Now that we have a working `jupyterlab` startup script, we can create a user service for it. I use `systemctl` for this.

First, create the directory `systemctl` looks in for user-defined services:

```bash
$ mkdir -p ~/.config/systemd/user/
```

Edit the file `~/.config/systemd/user/jupyterlab.service` to look like this:

```conf
[Unit]
Description=Service to run JupyterLab in user space

[Service]
ExecStart=/bin/bash -c "source ~/.bash_profile; eval ~/jupyterlab.sh"

[Install]
WantedBy=default.target
```

Now enable the `jupyterlab.service` you just created with

```bash
$ systemctl --user enable jupyterlab.service
```

> **Note:** If you get a message like `Failed to connect to bus: No such file or directory`, it means the `dbus` service isn't running for your user. Restarting, or starting a new shell should do the trick. I had this issue when logging in as a new user using the `su - <user name>` command.

Start the service with the `start` command, and view the status of the service with the `status` command:

```bash
$ systemctl --user start jupyterlab
$ systemctl --user status jupyterlab
```

> **Note:** If you don't see any log messages when using the `status` command, you may need to edit `/etc/systemd/journald.conf` as root and set the value `Storage=persistent`, per [this issue](https://stackoverflow.com/questions/30783134/systemd-user-journals-not-being-created/47930381#47930381)

The output from `status` will tell you the address to visit and help you with troubleshooting steps as well. If you still are using tokens (instead of a password, as described above), the token will be displayed in this log. 

That's it! You now have a service that automatically starts that will give you remote access to Jupyter Lab.

## Bonus - Jupyter Lab Widget Support

To enable `ipywidgets` widget support with `jupyterlab`, run the following command from your `anaconda` environment:

```bash
(anaconda3-5.0.1) $ conda install -c conda-forge ipywidgets
(anaconda3-5.0.1) $ jupyter labextension install @jupyter-widgets/jupyterlab-manager
```


![Python Logo](/assets/images/posts/2017/python_logo.svg)
![Anaconda Logo](/assets/images/posts/2017/anaconda_logo.png)
![Node.js Logo](/assets/images/posts/2017/node_logo.png)