---
title: "Mac Big Sur Virtual Machine on a Windows 10 VirtualBox Host"
date: 2021-12-08T23:09:07-05:00
featured_image: "/images/20211208_Mac-Big-Sur-VirtualBox-Windows.png"
description: "Mac Big Sur Virtual Machine on a Windows 10 Host using VirtualBox Hypervisor"
categories: ["virtualization"]
tags: ["virtualization", "macOS", "VirtualBox"]
draft: false
---

## New Temporary Job
I’ve recently got a new temporary position working at a different office. My supervisor mentioned that the office used Mac machines. To try and get ahead of the game, I decided that I should start doing some work on a mac. The only problem is, I don’t have one. And I’m not sure I’m ready to fork out the money for one (if this temporary position turns out to be a permanent one, maybe we can talk).

## So my next step is VIRTUALIZATION

I successfully have built a Mac Big Sur (Big Sur is the version of the Mac Operating system, MacOS) machine and have actually developed this site on it. I told my kids about it and my son asked me how I did it. This is my attempt at explaining what virtualization is and how I went about setting up my virtual Mac.

The simplest definition of virtualization or a virtual machine (VM) that I could find is _a program on a computer that works like it is a separate computer inside the main computer. The program that controls virtual machines is called a hypervisor and the computer that is running the virtual machine is called the host._ This definition from [https://kids.kiddle.co](https://kids.kiddle.co/Virtual_machine). If you want to get a higher level of understanding check out https://www.ibm.com/cloud/learn/virtualization-a-complete-guide or https://en.wikipedia.org/wiki/Virtualization 

I’m going to use a Type-2 or “hosted” hypervisor (https://en.wikipedia.org/wiki/Hypervisor), specifically one called VirtualBox. 

# Mac Big Sur VM on a Windows 10 host
## VirtualBox Set Up

[Download Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads) (version at time of writing this is 6.1.30) and the VirtualBox 6.1.30 Extension Pack.

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_00-Download-VirtualBox.png" link="/images/20211208_Mac-VirtualBox-Windows_00-Download-VirtualBox.png" title="Download Oracle VM VirtualBox and Extension pack" alt="Screenshot of VirtualBox Download Screen" >}}

After VirtualBox is downloaded, double click the downloaded file and click next on the screens to install with the default settings. After the installation, open the program and we'll install the extention pack.

Open the installed VirtualBox program and click Preferences (the tools icon) to open up the settings. Click the Extensions and then click the folder icon on the right with the green plus icon (+). Select the Extentions file that you downloaded from the Virtualbox website. Click install and agree to the VirtualBox agreement. After the Extentions install click "Ok" to get back to the Oracle VM VirtualBox Manager Window.

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_01-Install-Extention-Pack.png" link="/images/20211208_Mac-VirtualBox-Windows_01-Install-Extention-Pack.png" title="Installing Oracle VM VirtualBox Extension pack" alt="Screenshot of Installing Oracle VM VirtualBox Extension pack" >}}


## Create a New Virtual Machine
Click *New* at the top left corner. See screenshot below.

### Select the Name and Type of Virtual Machine
I provided mine MacOSBigSur
The Big Sur version is actually MacOS 11 but we select the _Mac OS X (64-bit)_ as it is a base Mac OS 64 bit. [More on the MacOS operating system here](https://en.wikipedia.org/wiki/Macintosh_operating_systems) and [more on the MacOS releases here](https://en.wikipedia.org/wiki/MacOS_version_history#Releases)).

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_02-New-VM.png" link="/images/20211208_Mac-VirtualBox-Windows_02-New-VM.png" title="Selecting name and OS type" alt="Screenshot of Naming and selecting OS Screen" >}}


### Virtual Machine Memory Size
Your virtual machine will be using your computer's or host machine's memory. [Windows 11 requirements](https://docs.microsoft.com/en-us/windows/whats-new/windows-11-requirements) states that it needs 4 gigabytes (GB) of RAM or greater. Your host machine should have more RAM if you will be running Virtual Machines. (Yes, you can run more than one simultaneously). Apple's [macOS Big Sur technical specification](https://support.apple.com/kb/sp833) says Big Sur requires 4 GB of RAM for Big Sur. [VirtualBox documentation](https://www.virtualbox.org/wiki/End-user_documentation) suggests that _at least 512 MB of RAM (but probably more, and the more the better)._ This is the sole VM I will be running, so going with the *more is better* idea, (and I have 16GB to work with on my host), I'm going to go with 6144 MB which is approximately 6 GB.

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_03-Memory-size.png" link="/images/20211208_Mac-VirtualBox-Windows_03-Memory-size.png" title="Selecting memory size for VM" alt="Screenshot of VM memory size screen" >}}

We will create a virtual hard disk now. Notice the recommended size of the hard disk says 8.00 GB this is pretty small. Especially since Mac says [you will need 35.5GB storage available for Big Sur](https://support.apple.com/kb/sp833). Keep in mind as the RAM used in the virtual machine is taken from the host, the same goes for the hard disk space. Your Windows host machine should have a large hard disk that the virtual machine can use (60 GB at least). But we’ll set the size later.

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_04-Hard-Disk.png" link="/images/20211208_Mac-VirtualBox-Windows_03-Memory-size.png" title="Selecting memory size for VM" alt="Screenshot of VM memory size screen" >}}

The hard disk file type is going to be VHD (Virtual Hard Disk). You can read [more information on the differences in hard disk file types](https://www.parallels.com/blogs/ras/vdi-vs-vhd-vs-vmdk/).

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_05-Hard-Disk-File-Type.png" link="/images/20211208_Mac-VirtualBox-Windows_03-Memory-size.png" title="Selecting memory size for VM" alt="Screenshot of VM memory size screen" >}}

As pointed out above your virtual machine should be at the very least 60 GB. And clicking “Create” will create your virtual machine.After the virtual machine is created, close VirtualBox.

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_06-Storage-On-Physical-Hard-Disk.png" link="/images/20211208_Mac-VirtualBox-Windows_03-Memory-size.png" title="Selecting memory size for VM" alt="Screenshot of VM memory size screen" >}}


{{< figure src="/images/20211208_Mac-VirtualBox-Windows_07-Launching-Big-Sur-Machine.png" link="/images/20211208_Mac-VirtualBox-Windows_07-Launching-Big-Sur-Machine.png" title="Lauching Virtual Mac" alt="Screenshot Virtual Mac start screen selecting language" >}}


The following commands will modify VituralBox to allow Big Sur image (.iso file) to be installed:

Click the Windows Start Menu and type cmd. Right click on the Command Prompt App and click Run as Administrator.
Then past the following commands:

{{< highlight bash "linenos=table,hl_lines=2,linenostart=1" >}}
cd "C:\Program Files\Oracle\VirtualBox\"
VBoxManage.exe modifyvm "MacOSBigSur" --cpuidset 00000001 000106e5 00100800 0098e3fd bfebfbff
VBoxManage setextradata "MacOSBigSur" "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "iMac11,3"
VBoxManage setextradata "MacOSBigSur" "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion" "1.0"
VBoxManage setextradata "MacOSBigSur" "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "Iloveapple"
VBoxManage setextradata "MacOSBigSur" "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"
VBoxManage setextradata "MacOSBigSur" "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC" 1

:: Only add the following line if you create virtual box on AMD CPU

VBoxManage modifyvm "MacOSBigSur" --cpu-profile "Intel Core i7-6700K"
{{< / highlight >}}


Launching BigSur machine, Installing Big Sur image
Open VirtualBox again and select the MacOSBigSur machine and click “Start” to launch the Big Sur virtual machine.

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_08-Start-Big-Sur-Virtual-Machine.png" link="/images/20211208_Mac-VirtualBox-Windows_08-Start-Big-Sur-Virtual-Machine.png" title="Start Big Sur Virtual Machine" alt="Starting Big Sur Virtual Machine screenshot" >}}

You then have to select the iso image the machine will start on. 

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_09-Select-macOS-ISO-Image.png" link="/images/20211208_Mac-VirtualBox-Windows_09-Select-macOS-ISO-Image.png" title="Select macOS ISO image" alt="Select macOS ISO Image screenshot" >}}

After clicking “Choose,” the machine will boot in recovery mode where we can select how to boot the operating system. Click on “English” and select the arrow to proceed .

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_10-Select-Big-Sur-Language.png" link="/images/20211208_Mac-VirtualBox-Windows_10-Select-Big-Sur-Language.png" title="Selecting Language" alt="Select Language screenshot" >}}

You will then get to the screen where the available volumes will be examined. We will be using the “Disk Utility” to format the drive to make sure it’s correctly formatted for the installation of macOS Big Sur image.

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_11-Disk-Utility.png" link="/images/20211208_Mac-VirtualBox-Windows_11-Disk-Utility.png" title="Using the Disk Utility" alt="Disk Utility screenshot" >}}

Select the virtual disk to format, “VBox HARDDISK Media” click the Erase menu item at the top of the screen. 

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_12-Disk-Format.png" link="/images/20211208_Mac-VirtualBox-Windows_12-Disk-Format.png" title="Format Disk" alt="Format Harddisk screenshot" >}}

Provide the volume a name and click “Erase”

When the format is complete click “Done” and close the window by clicking the red X at the top left of the screen.
We can now install the macOS Big Sur operating system. Select Install macOS Big Sur, click continue, and agree to the license agreement

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_14-Install-macOS-Big-Sur.png" link="/images/20211208_Mac-VirtualBox-Windows_14-Install-macOS-Big-Sur.png" title="Install macOS Big Sur" alt="Install macOS Big Sur screenshot" >}}

Select the disk where you want to install macOS and click continue

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_15-Select-Disk-To-Install-MacOS-Big-Sur.png" link="/images/20211208_Mac-VirtualBox-Windows_15-Select-Disk-To-Install-MacOS-Big-Sur.png" title="Select Disk To Install" alt="Screenshot of Installing macOS Big Sur on Disk" >}}


Once the installation completes, you’ll then be able to configure your machine as Apple products ask you to do upon first boot up. After this your machine is pretty much set up. 

## Conclusion

Setting up a virtual macOS Big Sur machine will allow you to really test out what it is like having a mac. Keep in mind it will not be as smooth as actually having a mac. I’m enjoying toying around with the machine and I am planning to get my own mac machine in the future. Hope you enjoyed this and maybe I’ll write up some things that got my virtual mac running even nicer. Until then!
