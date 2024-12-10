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

## Security

### enable security advisor

Will check out the sane and recommended by Synology configurations.

### disable default admin user

Common vector of attack.

### disable ssh

Common vector of attack, notably because only the admin users can use it.

### enable automatic updates for system

### enable automatic updates for services

### change the default ports

`Control Panel -> Login Portal`

### group users into groups

Easier to control policies and permissions and such.

### allow 2 factor authentication

### disable enhancing browser compatibility by skipping IP checking

`Security -> Security` under `General`

### set logout timer

`Security -> Security` under `Login Setitings`

### enable account protection

`Security -> Account` under `Account protection` should be enabled.

### enable adaptive MFA

`Security -> Account` under `Adaptive MFA`

### enable IP level auto blocking

`Security -> Protection` under `Auto Block`

### disable unneeded file services

`File Services -> *` whatever service is not desired

### permission viewer provides one in place overview

`Permission viewer`

### fine tune security advisor profile

### set static IP address

## Control panel

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

- [Synology NAS Compete Build, Setup, RAID, Pools and Volumes (2024 SETUP GUIDE #1) by NASCOMPARE](https://www.youtube.com/watch?v=TDV6uCH-4Ic)
- [COMPLETE BEGINNER’S GUIDE for Synology NAS - 2023 DSM 7.2 by SpaceRex](https://www.youtube.com/watch?v=T1xW97eyXB8)
- [How to Setup a Synology NAS for the first time in DSM 7 (Complete Guide for 2021+) by SpaceRex](https://www.youtube.com/watch?v=oWujGFVATiI)
- [Synology NAS Setup & Configuration Guide! by WunderTech](https://www.wundertech.net/synology-nas-initial-setup-ultimate-guide/)
