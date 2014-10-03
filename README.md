virtual-development-server
==========================

Repository with information on how to create a solid development server in a short amount of time

# TOC
<!-- MarkdownTOC depth=2 -->

- Development Server Installation
    - Requirements
    - Install VirtualBox
    - Create virtual machine (VirtualBox)
    - Ubuntu installation
    - Apache
    - /etc/hosts on Host
    - Need to know
    - Todo / wishlist

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
- ISO: mount Ubuntu image

## Ubuntu installation
Install Ubuntu by following the installation wizard and use defaults on most questions. When wizard ask what packages to (pre-)install, select the following:
- OpenSSH
- LAMP

Afterwards, login into system and install any updates and nfs-common (for the NFS share):
```Shell
$ sudo aptitude update
$ sudo aptitude upgrade
$ sudo aptitude install nfs-common
```

### Add NFS share
Add the NFS share to the guest by connecting to the host:
```Shell
$ sudo mkdir /mnt/share-name
```

Show which mounts are exposed on the host:
```Shell
# this IP is the default IP of the Host. This may vary depending on your Host-only network configuration in VirtualBox
$ showmount -e 192.168.56.1
```

Mount by adding to /etc/fstab:
```Shell
$ sudo vi /etc/fstab
# add line like:
192.168.56.1:/path/to/share /mnt/share-name nfs defaults 0 0
```

## Apache

### Enabled modules
- rewrite
- vhost_alias

### Use Dynamic Virtual hosts
See [this article](http://eosrei.net/articles/2012/08/create-dynamic-virtual-hosts-apache-http-vhostalias)

My current vhost:
```ApacheConf
<VirtualHost *:80>
    ServerAdmin xx@xx.xx
    ServerName dev.ubuntu1404
    ServerAlias *.dev.ubuntu1404

    <Directory /data/*/http>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Order allow,deny
        allow from all
        Require all granted
    </Directory>

    LogFormat "%V %h %l %u %t \"%r\" %s %b" vcommon
    CustomLog /data/logs/access.log vcommon
    ErrorLog /data/logs/error.log

    #URL structure project.dev.ubuntu1404
    UseCanonicalName Off
    VirtualDocumentRoot /data/%1/http/
</VirtualHost>
```

### ServerName
Open /etc/apache2/apache2.conf and add to the bottom of the file
```ApacheConf
ServerName ubuntu1404 # or anything else you like
```

Needless to say, you'll have to reload Apache to make changes effective: `$ sudo service apache2 reload`

## /etc/hosts on Host
Edit your /etc/hosts on your Host machine and add the following lines:
```Shell
192.168.56.101 ubuntu1404
192.168.56.101 dev.ubuntu1404
192.168.56.101 <project>.dev.ubuntu1404
```

## Need to know
- added 2 Network Interfaces
  - Host-Only (create one within VirtualBox): this one is used for local communication. Has static IP, so no worry when your IP changes while using multiple networks (home, office, etc.)
  - Bridged: this one is for internet communication (updates, API's, etc.)
- enable NFS on NAT on Mac Host: http://gathering.tweakers.net/forum/list_message/39054666#39054666
- Beware of DNS Rebinding trap: http://xs4all.ipv6.narkive.com/GOzl3Gdl/de-dns-van-fritzbox-7390-vindt-hosts-met-eigen-ipv6-nummers-niet-leuk
- On host: add `umask 002` to ~/.bash_profile. With this fix the files you create on your host are also Group writable, so you'll only have to share the group on the guest to be able to write to files.
- On guest: add user to www-data group and group of NFS share (could be 'dialout'), also added www-data to group of NFS share


## Todo / wishlist
- nginx
- varnish
- nodejs