virtual-development-server
==========================

Repository with information on how to create a solid development server in a short amount of time

# Development Server Installation

## Requirements:
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Ubuntu 14.04 LTS image](http://www.ubuntu.com/download/server)

## Need to know:
- added 2 Network Interfaces
  - Host-Only (create one within VirtualBox): this one is used for local communication. Has static IP, so no worry when your IP changes while using multiple networks (home, office, etc.)
  - Bridged: this one is for internet communication (updates, API's, etc.)
- enable NFS on NAT on Mac Host: http://gathering.tweakers.net/forum/list_message/39054666#39054666
- Beware of DNS Rebinding trap: http://xs4all.ipv6.narkive.com/GOzl3Gdl/de-dns-van-fritzbox-7390-vindt-hosts-met-eigen-ipv6-nummers-niet-leuk
- On host: add `umask 002` to ~/.bash_profile. With this fix the files you create on your host are also Group writable, so you'll only have to share the group on the guest to be able to write to files.
- On guest: add user to www-data group and group of NFS share (could be 'dialout')
