# Installation

# Enable Windows Subsystem for Linux

Open `PowerShell` as Administrator and run:

```bash
PS C:\Users\User> dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

## Check requirements for running WSL 2

For x64 systems: **Version 1903** or higher, with **Build 18362** or higher. If not, please use **Windows Update** or download and run[ **Windows Update Assistant** ](https://go.microsoft.com/fwlink/?LinkID=799445).


## Enable Virtual Machine feature

Open `PowerShell` as Administrator and run:

```bash
PS C:\Users\User> dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

## Download the Linux kernel update package

Download and install [WSL2 Linux kernel update package for x64 machines](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

## Set WSL 2 as your default version

```bash
PS C:\Users\User> wsl --set-default-version 2
```

## Install Ubuntu 18.04 LTS

Install it from [**Windows Store**](https://www.microsoft.com/zh-cn/p/ubuntu-1804-lts/9n9tngvndl3q?rtc=1&activetab=pivot:overviewtab)

## Install Docker Service

Follow the instructions [here ](https://docs.docker.com/engine/install/ubuntu/)and install docker service in WSL. After installation, as `systemd` is not enabled in WSL, please start the docker by running as below.

```bash
$ sudo service docker start
```

## Install Docker Desktop for Windows

Follow the instructions [here](https://docs.docker.com/docker-for-windows/install/). Please pay attention about the Windows system type \(Pro/Enterprise/Home, etc.\). The Windows 10 Home is quite different from others.
