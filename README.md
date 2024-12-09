# Synology DiskStation Manager

Notes of best practices found around to setup a Synology NAS for home
use. References included at the end. The guide is based on `DSM-7.2`.

## Initial setup

Follow the install instructions after locating the attached NAS on the same
network using [finds.synology.com](http://find.synology.com/).

## Storage manager

### Make volume

You should be greeted with prompt, do as you see wish or flow with the
defaults.

Leave the drive checking, it will take some time.

### Schedule drive tests

`HDD/SSD -> Settings`

- SMART quick test: every month
- SMART extended test: every three months

### Schedule scrubbing

`Storage -> Storage Pool N -> Volume K -> Schedule Data scrubbing`

- enable data scrubbing
- select storage pools
- adjust frequency or choose data scrubbing specific period
- select to run it later, as it is a new install

## Control panel

### Static IP address

`Network -> Network interfaces`

Edit and put address that is outside the DHCP scope.

Alternatively do it trough the router.

### Basic security

`Security -> Security` check most with exception of *Enhance browser compatibility...*

### Enable account protection

`Security -> Account` the `Account protection` tab is selected.

### Enable auto block and DoS

`Security -> Protection`

### Create shared folder

`Shared folder -> Create`

### Add users

`Users and groups -> User -> Create`

Be cautious of adding an user into admin group.

### Fast clone

`File services -> Advanced ` and then select `Fast clone`

### Bypass traverse checking

`File services -> Advanced ` and then select `Enable bypass traverser checking`

## Snapshot replication

Install the service trough `Package manager`.

### Schedule snapshot replication

`Snapshots -> Settings -> Schedule`

- daily at night
- 7 days retention policy

Can make replication.


## References

- [Complete Synology NAS Setup Guide (Zero to Hero) by Tech Me Out](https://www.youtube.com/watch?v=Clr04THD49g)
- [COMPLETE BEGINNER’S GUIDE for Synology NAS - 2023 DSM 7.2 by SpaceRex](https://www.youtube.com/watch?v=T1xW97eyXB8)
- [How to Setup a Synology NAS for the first time in DSM 7 (Complete Guide for 2021+) by SpaceRex](https://www.youtube.com/watch?v=oWujGFVATiI)
- [Synology NAS Setup & Configuration Guide! by WunderTech](https://www.wundertech.net/synology-nas-initial-setup-ultimate-guide/)
- [Synology NAS Compete Build, Setup, RAID, Pools and Volumes (2024 SETUP GUIDE #1) by NASCOMPARE](https://www.youtube.com/watch?v=TDV6uCH-4Ic)
