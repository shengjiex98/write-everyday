# NVIDIA GPU & Docker & WSL

## What software does NVIDIA provide?

- **NVIDIA GPU Driver**
- **NVIDIA CUDA toolkit**: enables the CUDA computing platform (this is what most ML libraries refer to when they talk about "using GPU"). Installation guide for [Ubuntu](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#ubuntu-installation) and [WSL](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#wsl-installation).
- **NVIDIA container tookit**: enables containers to access GPU. For docker, there's a package [`nvidia-docker2`](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) that covers all the packages one would need to work with Docker, [including](https://github.com/NVIDIA/nvidia-docker/issues/1268#issuecomment-632692949)
  - nvidia-container-toolkit
  - nvidia-container-runtime
  - libnvidia-container

## To make WSL work with NVIDIA GPU

Nothing special needs to be done. Generally, using the latest versions of Windows/NVIDIA driver/WSL2 means you already have GPU access in WSL. Here's the specifics:

1. Update Windows to [Windows 11](https://microsoft.com/software-download/windows11) or [Windows 10, 21H2](https://microsoft.com/software-download/windows10)
2. Install an [NVIDIA CUDA-enabled driver for WSL](https://developer.nvidia.com/cuda/wsl). Note that this driver is installed **on the host Windows OS**, not in WSL.
3. [Set up WSL2](https://docs.microsoft.com/en-us/windows/wsl/install). Make sure the kernel version is 5.10.43.3 or higher. To check kernel version, run the following command in the WSL terminal:
    ```bash
    $ cat /proc/version
    ```
    If you need to update the kernel, run the following command **in PowerShell**:
    ```PowerShell
    wsl.exe --update
    ```

That's it. Open up a WSL shell and verify that GPU is recognized
```
$ nvidia-smi
Sat Jan 22 20:11:36 2022
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 510.00       Driver Version: 510.06       CUDA Version: 11.6     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
...
```

Optionally, if you want to use CUDA within WSL, then the NVIDIA CUDA toolkit must be installed **on WSL**. Make sure to not install the normal `cuda` package since it includes an NVIDIA driver for Linux that will conflict with the Windows driver we installed in step 2. Refer to [here](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#wsl-installation) for how to correctly set up CUDA. Alternatively, use `conda` to install CUDA alongside ML frameworks.

## Enable GPU in Docker (Linux only)

*Note: this section applies to native Linux OS, not WSL*

Refer to the [official NVIDIA doc](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html). Here's a quick summary:

1. Install NVIDIA driver on the **host system**. On Ubuntu, this can be done through `Additional Drivers` pre-installed with the system.
2. Install [Docker](https://docs.docker.com/engine/install/ubuntu/) (19.03 or newer). You might want to consider [add your user to the `docker` group](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user), as it will make things easier when using ade (otherwise you'll need to use `sudo` with ade commands).
3. Install NVIDIA Container Toolkit, which enables docker containers to use Nvidia GPUs on the host system. This can be installed through the [`nvidia-docker2`](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) package.

## Enable GPU in Docker containers run in WSL

This is where things get messy. There are conflicting information in the docs such as whether to install the `nvidia-docker2` package in WSL. Preliminary testing shows that running `docker run --gpus=all` in WSL works out-of-box without installing `nvidia-docker2`, but more testing needs to be done before this section can be more useful.
