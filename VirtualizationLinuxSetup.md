# Virtualisation & Linux Setup

Instead of VMWare or VirtualBox, I have decided to use a hypervisor I am more familiar with, Virtual Machine Manager which uses libvirt that runs on QEMU/KVM of my existing linux laptop.

![Virtual Machine Manager](Screenshots/VirtManager.png)

I proceeded to install the ISO for Ubuntu 24.04 that was provided.

![UbuntuISO.png](./Screenshots/UbuntuISO.png)

I allocated 4 cores and 4 GiB of memory to the Guest OS.

![GuestResources.png](Screenshots/GuestResources.png)

Network settings are VirtManager default, which is via NAT.

![GuestNIC.png](./Screenshots/GuestNIC.png)

After running through the installation steps, I quickly had a working desktop.

![UbuntuFirstLaunch.png](Screenshots/UbuntuFirstLaunch.png)
