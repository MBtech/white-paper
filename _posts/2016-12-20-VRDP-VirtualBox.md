---
layout: post
title: "VRDP VirtualBox"
categories:
  - tutorial
tags:
  - content
---

## Using VRDP with VirtualBox VMs
Recently, I had some Android VMs running on a remote server and I had to look at the VM state graphically to see the progress of a process.
VirtualBox provides VirtualBox Remote Desktop Extension (VRDE) which lets you have a remote display of a VM. Quoting VirtualBox's website:

``VirtualBox can display virtual machines remotely, meaning that a virtual machine can execute on one computer even though the machine will be displayed on a second computer, and the machine will be controlled from there as well, as if the virtual machine was running on that second computer.``

Oracle provides support for the VirtualBox Remote Display Protocol (VRDP) in such a VirtualBox extension package. When this package is installed, VirtualBox versions 4.0 and later support VRDP the same way as binary (non-open-source) versions of VirtualBox before 4.0 did.

You can install this extension by downloading and installing the extension pack that matches your virtual box version from http://download.virtualbox.org/virtualbox/

It would be named with a ``.vbox-extpack`` extension.

Even when the extension is installed, the VRDP server is disabled by default. It can easily be enabled on a per-VM basis either in the VirtualBox Manager in the "Display" settings or with VBoxManage:

``VBoxManage modifyvm "VM name" --vrde on``

By default, the VRDP server uses TCP port 3389. But if you want to have multiple machines running with VRDP enabled, you need to specify a port or a set or ports manually and that can be done using:

``VBoxManage modifyvm "VM name" --vrdeport 5000,5010-5012``

The list of open ports could be comma separated or ranged as shown above. VRDP server will bind to one of the ports available.

The actual port that VM will be using can be seen through

``VBoxManage showvminfo``

You can use any third party RDP viewer to access the remote display
https://www.virtualbox.org/manual/ch07.html#rdp-viewers

**References**
- https://www.virtualbox.org/manual/ch07.html
