Docker lets you manage virtual machines built just for running software. These virtual machines will have the same OS version, all the libraries to run the software already installed, etc.

Pros of running software through Docker:
- You have everything you need: everything you need to run the software is already installed and built into the virtual machine
- Better for sharing: same virtual machine, same OS version, same libraries, same experience
- Cleanup is easier: if you remove the virtual machine, everything else is removed

In order for this to work though, you need to install Docker. Windows cannot run docker images by itself.

Cons of running software through Docker:
- Performance will be worse: you're still running software through a virtual machine
- More storage: installing the OS, all the libraries, etc.

You'll usually see Docker used for software that isn't too CPU/GPU intensive, and software that will be used by developers and tech savvy people.

### Docker Desktop
If you are:
- using windows
- want to run a container in Windows Subsystem for Linux (WSL)

### Docker and Visual Studio
If you use a DevContainer with Visual Studio, make sure you have the (Workspace) and the DevContainer 