**Installing Docker in Windows**

This guide will provide step-by-step instructions on how to install Docker on Windows using the Docker Desktop Installer. Docker Desktop for Windows is the Community version of Docker for Microsoft Windows.

## Prerequisites

1. Make sure that your system meets the following requirements:

    - Windows 10 64-bit: Pro, Enterprise, or Education (Build 16299 or later).
    - Hyper-V and Containers Windows features must be enabled.
    - The following hardware prerequisites are required to successfully run Client Hyper-V on Windows 10:
        - 64 bit processor with Second Level Address Translation (SLAT)
        - 4GB system RAM
        - BIOS-level hardware virtualization support must be enabled in the BIOS settings. 

2. Docker Desktop requires Microsoft Hyper-V to run. The Docker Desktop Windows installer enables Hyper-V for you, if needed, and restarts your machine. 

## Steps

**Step 1**: Download Docker Desktop for Windows

First, visit the Docker website to download Docker Desktop for Windows. 

[Official Docker Download Link](https://www.docker.com/products/docker-desktop)

**Step 2**: Install Docker Desktop on Windows

Once the Docker Desktop Installer.exe is downloaded, double-click to run the installer.

**Step3**: Follow the instructions on the installation wizard to accept the license, authorize the installer, and proceed with the install.

**Step 4**: When prompted, authorize the Docker Desktop Installer with your system password during the install process. 

**Step 5**: Click "Finish" on the setup complete dialog to launch Docker Desktop.

## Docker Desktop does not start automatically after installation. 

**Step 6**: To run Docker Desktop, search for Docker, and select Docker Desktop in the search results.

**Step 7**: When the whale icon in the status bar stays steady, Docker Desktop is up-and-running, and accessible from any terminal window.

**Step 8**: If you would like to check the Docker version, open a terminal window and type `docker --version`.

## Installing Docker through PowerShell 

If you prefer PowerShell, you can also install Docker through the Windows PowerShell.

**Step 1**: Open Windows PowerShell as an administrator.

**Step 2**: Run the following command to download Docker's installation script:

```powershell
Invoke-WebRequest -UseBasicParsing -Uri https://get.docker.com/ -OutFile docker-install.ps1
```

**Step 3**: Run the Docker installation script with the following command:

```powershell
.\docker-install.ps1
```

**Step 4**: Docker should now be installed. You can check by running:

```powershell
docker --version
```

Now, Docker is installed and ready to be used on Windows. 

*Note: This guide is for Windows 10 64-bit: Pro, Enterprise, or Education (Build 16299 or later) machines.*
