## Parallels Desktop
Parallels Desktop is a software that provides hardware virtualization for Macintosh computers with Intel processors. Parallels Desktop for Mac is a hardware emulation virtualization software, using hypervisor technology that works by mapping the host computer's hardware resources directly to the virtual machine's resources. This allows the virtual machine to operate identically to a standalone computer system.

Here are the steps to install Parallels Desktop on your Mac:

1. Download Parallels Desktop. Visit the Parallels Desktop webpage and click on Download Now.

2. Double click on the downloaded file.

3. Follow the on-screen instructions to install the software.

4. Once installed, open Parallels Desktop.

5. Now, you can either create a new virtual machine or get Windows 10 from Microsoft. If you already have a Windows .iso file, you can use that.

6. Follow the prompts to complete the installation of your virtual machine.

7. Once installation is complete, you can start using your virtual machine!

Remember: Using Parallels Desktop requires a considerable amount of your Mac's resources, so make sure your Mac has enough RAM and storage space to handle it. For best performance, a Mac with an Intel Core i5 Processor, 8GB RAM, and at least 50GB of storage is recommended for running Parallels Desktop. 

It's also important to note that Parallels Desktop is not a free software. It offers a 14-day free trial, after which you need to purchase a subscription to continue using it. It offers different types of subscriptions depending on whether it is for personal, business or educational use.



## WSL2
WSL2 (Windows Subsystem for Linux version 2) can function as a type of virtual machine within the Windows operating system. 

WSL2 is an upgrade from WSL and offers significant improvements such as a real Linux kernel, more system call compatibility, and the ability to use Docker natively. It provides a full compatibility layer for running Linux binary executables natively on Windows 10.

WSL2 is not a traditional VM experience. Instead of dealing with a virtual machine interface, you can use WSL2 more seamlessly with the Windows system. You can access your local network, files, and even start up services automatically. The integration with the Windows environment means you can run Linux commands alongside Windows commands.

Here's how you can install it:

1. Open PowerShell as Administrator and run:
   
   ```wsl --set-default-version 2```

2. To install a new distro, go to Microsoft Store and choose your favorite Linux distribution.
   
Please note, before installing WSL 2, you must enable the Virtual Machine Platform optional feature, and to do this, the command is: 

```dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart```

Also, make sure your Windows 10 version is updated to at least version 1903 or higher for a smooth installation and usage of WSL2. 

After setting up, you can open the Linux terminal from your start menu or by simply typing `wsl` or the name of your Linux distribution (like `ubuntu`) in the PowerShell or Command Prompt.

Lastly, remember to check your BIOS settings to ensure virtualization is enabled on your machine.
