# Autoware.Auto setup

This document contains instructions on setting up [Autoware.Auto](https://gitlab.com/autowarefoundation/autoware.auto/AutowareAuto) on Ubuntu 20.04, as well as a few helpful tips to quality-of-life improvements.

- [Autoware.Auto setup](#autowareauto-setup)
  - [Overview](#overview)
  - [Installation](#installation)
    - [ADE](#ade)
    - [Set up `adehome` and config files](#set-up-adehome-and-config-files)
    - [Download ADE images & enter dev environment](#download-ade-images--enter-dev-environment)
    - [Launch non-default environments](#launch-non-default-environments)
  - [Next steps](#next-steps)
  - [Tips](#tips)

## Overview

Autoware.Auto utilizes a containerized environment to make setting up easier as well as more reliable. To achieve this it uses a tool called [ADE](https://ade-cli.readthedocs.io/en/latest/), which is essentially a wrapper for Docker with GitLab integration that enables easy [*volume mounting*](https://ade-cli.readthedocs.io/en/latest/intro.html#terminology).

A few prerequisites need to be met before proceeding. Namely, the setup requires:
1. An Nvidia GPU with appropriate driver installed on the **host system**. On Ubuntu, this can be done through `Additional Drivers` pre-installed with the system.
2. [Docker](https://docs.docker.com/engine/install/ubuntu/) (19.03 or newer). You might want to consider [add your user to the `docker` group](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user), as it will make things easier when using ade (otherwise you'll need to use `sudo` with ade commands).
3. Nvidia Container Toolkit, which enables docker containers to use Nvidia GPUs on the host system. This can be installed through the [nvidia-docker2](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) package.

Once the prerequisites are met, we can move on the the following steps.

## Installation

### ADE

For system-wide installation, it is recommended to install to `/usr/local/bin`, which is in `$PATH` by default. Alternatively, it can be installed to `~/.local/bin` for one-user install. Below is an example for `x86_64` installed to `/usr/local/bin` (You might need sudo for some of the commands)
```bash
$ cd /usr/local/bin
$ wget https://gitlab.com/ApexAI/ade-cli/-/jobs/1859684348/artifacts/raw/dist/ade+x86_64
$ mv ade+x86_64 ade
$ chmod +x ade
$ ./ade --version
4.4.0
$ ./ade update-cli
$ ./ade --version
<latest-version>
```
You can find all ADE binary files [here](https://gitlab.com/ApexAI/ade-cli/-/releases).

Verify that ADE is correctly installed:
```bash
$ which ade
/usr/local/bin/ade
$ ade --version
<version>
```

### Set up `adehome` and config files

> ADE needs a directory on the host machine which is mounted as the user's home directory within the container. The directory is populated with dotfiles, and must be different than the user's home directory outside of the container. In the event ADE is used for multiple, projects it is recommended to use dedicated `adehome` directories for each project.
>
> ADE looks for a directory containing a file named `.adehome` starting with the current working directory and continuing with the parent directories to identify the ADE home directory to be mounted. ([ADE project page](https://ade-cli.readthedocs.io/en/latest/usage.html#ade-home))

Let's create a directory to be used as the home directory for ADE

```bash
# It doesn't not have to be called "adehome". As long as
# an .adehome file exist within the directory it is ok.
$ mkdir -p ~/adehome
$ cd ~/adehome
$ touch .adehome
```

Next, clone the Autoware.Auto repo:
> For ADE to function, it must be properly configured. Autoware.Auto provides an `.aderc` file which is expected to exist in the current working directory, or in any parent directory. Additionally, default configuration values can be overridden by setting environment variables. See the `ade --help` output for more information about using environment variables to define the configuration. ([Autoware.Auto project page](https://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/installation-ade.html))

```bash
$ cd ~/adehome
$ git clone https://gitlab.com/autowarefoundation/autoware.auto/AutowareAuto.git
```

> Checkout the latest release by checking out the corresponding tag or release branch. Alternatively, when not checking out any specific tag, the latest master branch will be used which may include features that are still being developed. For example: ([Autoware.Auto project page](https://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/installation-ade.html))
```bash
$ cd AutowareAuto
$ git checkout tags/1.0.0 -b release-1.0.0
```

However, at the moment of writing (2022-01-19) the `release-1.0.0` version of Autoware.Auto no longer works with the SVL Simulator. So we will stay in the `master` branch for now.

### Download ADE images & enter dev environment

Now we have all the required configuration files inside the `~/adehome/AutowareAuto/` directory, we can use ADE to automatically download required images (that includes libraries such as `ROS` and `SVL Simulator`).
```bash
$ cd ~/adehome/AutowareAuto
# This downloads & starts the default environment
$ ade start --update --enter
```

After ADE finishes downloading images, you should enter the ADE development environment automatically. You can verify by checking the shell prompt:
```bash
<username>@ade:~ $
```
And that means you've successfully set up Autoware! If you want to return to your local shell, type
```bash
<username>@ade:~ $ exit
```
should do the trick. Once you finish experimenting with it, you can run
```bash
# This command is NOT run in the ADE environment
$ ade stop
```
in you **host** terminal to shutdown the ADE environment.

### Launch non-default environments
There are several preconfigured environments to choose from (e.g. if you want to use the SVL simulator) by specifying an `.aderc` file. To see what is available, run

```bash
$ ls -l .aderc*
```

To launch with a configuration file other than the default, run 
```bash
$ ade --rc .aderc-amd64-foxy  start --update --enter
```

## Next steps

- Check out more details in [Autoware.Auto docs](https://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/installation-ade.html).
- Set up the SVL Simulator. You can start from the "Launching the simulator" section on [this page](https://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/lgsvl.html), as we have taken care of the prerequisites and ADE.
- Finally, try the [autonomous valet parking demo](https://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/avpdemo.html). Note: it might not be obvious from the documation, but the "Initializing the localization" part will not immediately work until the simulation is **running** (by pressing the "play" button in the simulator).

## Tips

- To monitor system GPU usage, VRAM usage, GPU temp etc., use `nvidia-smi`. To enable continuous monitoring: 
    ```bash
    $ watch -n 1 nvidia-smi
    ```
    Which will update related info every second.
- To monitor CPU usage, use `top`. Additionally, press `Shift` + `i` while top is running to change CPU usage to be the percentage of all cores.
