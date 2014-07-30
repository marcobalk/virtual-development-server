virtual-development-server
==========================

Repository with information on how to create a solid development server in a short amount of time

# TOC
<!-- MarkdownTOC depth=2 -->

- Development Server Installation
	- Requirements
	- Install VirtualBox
	- Create virtual machine (VirtualBox)
	- Need to know

<!-- /MarkdownTOC -->


# Development Server Installation

## Requirements
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Ubuntu 14.04 LTS image](http://www.ubuntu.com/download/server)

## Install VirtualBox
Just install VirtualBox like you install other software. If this is not enough information, please read the [VirtualBox manual](https://www.virtualbox.org/manual/UserManual.html)

### Create Host-only network
Before creating a virtual machine, open the VirtualBox settings and go to the Network tab. There you can create NAT networks as well as Host-only networks. For my virtual machines I use a Host-only network, so create one (default named 'vboxnet0').

## Create virtual machine (VirtualBox)
Below are some typical settings I use. Please feel free to use alternate values.

### General
- Type: Linux
- Version: Ubuntu (64 bit)

### System
- Memory: 1024MB

### Storage
- HDD: 16GB

## Need to know
- added 2 Network Interfaces
  - Host-Only (create one within VirtualBox): this one is used for local communication. Has static IP, so no worry when your IP changes while using multiple networks (home, office, etc.)
  - Bridged: this one is for internet communication (updates, API's, etc.)
- enable NFS on NAT on Mac Host: http://gathering.tweakers.net/forum/list_message/39054666#39054666
- Beware of DNS Rebinding trap: http://xs4all.ipv6.narkive.com/GOzl3Gdl/de-dns-van-fritzbox-7390-vindt-hosts-met-eigen-ipv6-nummers-niet-leuk
- On host: add `umask 002` to ~/.bash_profile. With this fix the files you create on your host are also Group writable, so you'll only have to share the group on the guest to be able to write to files.
- On guest: add user to www-data group and group of NFS share (could be 'dialout')
