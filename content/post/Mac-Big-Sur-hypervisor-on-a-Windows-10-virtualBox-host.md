---
title: "Mac Big Sur Hypervisor on a Windows 10 VirtualBox Host"
date: 2021-12-08T23:09:07-05:00
featured_image: "/images/20211208_Mac-Big-Sur-VirtualBox-Windows.png"
description: "Mac Big Sur Virtual Machine on a Windows 10 Host using VirtualBox Hypervisor"
categories: ["virtualization"]
tags: ["virtualization", "macOS", "VirtualBox"]
draft: true
---

## New Temporary Job
I’ve recently got a new temporary position working at a different office. My supervisor mentioned that the office used Mac machines. To try and get ahead of the game, I decided that I should start doing some work on a mac. The only problem is, I don’t have one. And I’m not sure I’m ready to fork out the money for one (if this temporary position turns out to be a permanent one, maybe we can talk).

## So my next step is VIRTUALIZATION

I successfully have built a Mac Big Sur (Big Sur is the version of the Mac Operating system, MacOS) machine and have actually developed this site on it. I told my kids about it and my son asked me how I did it. This is my attempt at explaining what virtualization is and how I went about setting up my virtual Mac.

The simplest definition of virtualization or a virtual machine (VM) that I could find is _a program on a computer that works like it is a separate computer inside the main computer. The program that controls virtual machines is called a hypervisor and the computer that is running the virtual machine is called the host._ This definition from [https://kids.kiddle.co](https://kids.kiddle.co/Virtual_machine). If you want to get a higher level of understanding check out https://www.ibm.com/cloud/learn/virtualization-a-complete-guide or https://en.wikipedia.org/wiki/Virtualization 

I’m going to use a Type-2 or “hosted” hypervisor (https://en.wikipedia.org/wiki/Hypervisor), specifically one called VirtualBox. 

# Mac Big Sur VM on a Windows 10 host

[Download Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads) (version at time of writing this is 6.1.30)
And open VirtualBox after it finishes installing.

## Create a New Virtual Machine
Click *New* at the top left corner. See screenshot below.

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_01-New-VM.png" link="/images/20211208_Mac-VirtualBox-Windows_01-New-VM.png" title="Creating a new hypervisor in VirtualBox" alt="Screenshot of Oracle VM VirtualBox Manager screen" >}}

## Select the Name and Type of Virtual Machine
I provided mine MacOSBigSur
The Big Sur version is actually MacOS 11 but we select the _Mac OS X (64-bit)_ as it is a base Mac OS 64 bit. [More on the MacOS operating system here](https://en.wikipedia.org/wiki/Macintosh_operating_systems) and [more on the MacOS releases here](https://en.wikipedia.org/wiki/MacOS_version_history#Releases)).

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_02-Name-And-Operating-System.png" link="/images/20211208_Mac-VirtualBox-Windows_02-Name-And-Operating-System.png" title="Selecting name and OS type" alt="Screenshot of Naming and selecting OS Screen" >}}

## Virtual Machine Memory Size
Your virtual machine will be using your computer's or host machine's memory. [Windows 11 requirements](https://docs.microsoft.com/en-us/windows/whats-new/windows-11-requirements) states that it needs 4 gigabytes (GB) of RAM or greater. Your host machine should have more RAM if you will be running Virtual Machines. (Yes, you can run more than one simultaneously). Apple's [macOS Big Sur technical specification](https://support.apple.com/kb/sp833) says Big Sur requires 4 GB of RAM for Big Sur. [VirtualBox documentation](https://www.virtualbox.org/wiki/End-user_documentation) suggests that _at least 512 MB of RAM (but probably more, and the more the better)._ This is the sole VM I will be running, so going with the *more is better* idea, (and I have 16GB to work with on my host), I'm going to go with approximately 12 GB.

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_03-Memory-size.png" link="/images/20211208_Mac-VirtualBox-Windows_03-Memory-size.png" title="Selecting memory size for VM" alt="Screenshot of VM memory size screen" >}}


{{< figure src="/images/20211208_Mac-VirtualBox-Windows_04-Hard-Disk.png" link="/images/20211208_Mac-VirtualBox-Windows_03-Memory-size.png" title="Selecting memory size for VM" alt="Screenshot of VM memory size screen" >}}

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_05-Hard-Disk-File-Type.png" link="/images/20211208_Mac-VirtualBox-Windows_03-Memory-size.png" title="Selecting memory size for VM" alt="Screenshot of VM memory size screen" >}}

{{< figure src="/images/20211208_Mac-VirtualBox-Windows_06-Storage-On-Physical-Hard-Disk.png" link="/images/20211208_Mac-VirtualBox-Windows_03-Memory-size.png" title="Selecting memory size for VM" alt="Screenshot of VM memory size screen" >}}

Launching BigSur machine


{{< figure src="/images/20211208_Mac-VirtualBox-Windows_07-Launching-Big-Sur-Machine.png" link="/images/20211208_Mac-VirtualBox-Windows_07-Launching-Big-Sur-Machine.png" title="Lauching Virtual Mac" alt="Screenshot Virtual Mac start screen selecting language" >}}