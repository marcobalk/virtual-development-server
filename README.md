virtual-development-server
==========================

Repository with information on how to create a solid development server in a short amount of time

# Development Server Installation

## Requirements:
- VirtualBox
- Ubuntu 14.04 LTS image

## Need to know:
- added 2 Network Interfaces
-- Host-Only (create one within VB)
-- Bridged
- enable NFS on NAT on Mac Host: http://gathering.tweakers.net/forum/list_message/39054666#39054666
- Beware of DNS Rebinding trap: http://xs4all.ipv6.narkive.com/GOzl3Gdl/de-dns-van-fritzbox-7390-vindt-hosts-met-eigen-ipv6-nummers-niet-leuk
- On host: add `umask 002` to ~/.bash_profile
- On guest: add user to www-data group and group of NFS share (could be 'dialout')