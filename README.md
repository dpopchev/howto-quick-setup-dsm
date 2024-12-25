# Synology DiskStation Manager

Rules of thumb about configuring an Synology NAS running `DiskStation` version
`7.2`. Find references at the end.

## Install

Follow the install instructions after locating the attached NAS on the same
network using [finds.synology.com](http://find.synology.com/).

## Storage

### schedule task to empty recycle bin

`Control Panel -> Task Scheduler -> Create`

Rules of thumb:

- empty bins every single night
- setup retention policy of 7 days

### schedule SMART HDD tests

`Storage Manager -> HDD/SDD -> Task Scheduler`

Rules of thumb:

- SMART quick test: every month
- SMART extended test: every six months

### schedule pool scrubbing

`Storage Manager -> Storage -> Pool -> Schedule Data Scrubbing`

Rules of thumb:

- make it every three months
- select to run it later, as it is a new install

### keep copy of old data around

Rules of thumb:

- keep old data around, ideally do a backup using `Hyper Backup`
- new HDDs either likely die in first few weeks or then likely last for long time
- it is not uncommon for HDDs in the same series to share same defect

### install exFAT access package

Rules of thumb:

- file system that all OS can read
- suitable to make external HDD and `Hyper Backup` important data

### install Snapshot Replication package

Rules of thumb:

- **note** should be enabled on every newly created shared folder
- enable snapshot per shared folder every 2 hours
- retain all snapshots for 7 days
- retain last day snapshot for 30 days
- **note** that retain policies will constrain how quickly spaces gained back,
e.g. 30 days after deletion

### install Hyper Backup package

Rules of thumb:

- schedule backup per shared folder or package and to other NAS or external drive

### install Storage Analyzer package

Rules of thumb:

- run reports to catch duplication files

## Security

### enable auto block

`Control Panel -> Security -> Protection -> Auto Block`

Rule of thumbs:

- Log attempts 10
- block for 5 minutes

### SMB settings

`Control Panel -> File Services -> SMB -> Advanced settings`

Rules of thumb:

- **do not** use SMB1 as there is security vulnerability
- **disable** `NLTMv1`

### disable AFP

`Control Panel -> File Sevices -> AFP`

Rules of thumb:

- **disable** as Apple has deprecated it

### disable NFS

`Control Panel -> File Sevices -> NFS`

Rules of thumb:

- **disable** as it uses IP, bot user, authentication

### disable FTP

`Control Panel -> File Sevices -> FTP`

Rules of thumb:

- **disable** unless needed

### disable default admin user

`Control Panel -> User &  Group`

Rules of thumb:

- **disable** the account named "admin"

### disable ssh

`Control Panel -> Terminal & SNMP`

Rules of thumb:

- **disable** leave close unless needed

### enable adaptive MFA

`Control Panel -> Security -> Account`

Rules of thumb:

- **enable** adaptive MFA
- advisable to **enable** 2-factor authentication

### disable firewall

`Control Panel -> Security -> Firewall`

Rules of thumb:

- advisable to leave disabled unless more complicated configuration is needed

### enable security advisor

Will check out the sane and recommended by Synology configurations.

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

### disable unneeded file services

`File Services -> *` whatever service is not desired

### permission viewer provides one in place overview

`Permission viewer`

### fine tune security advisor profile

### set static IP address

### enable DoS protection

## Users and groups

### enable user home service

### create group

Better to attach password settings to groups.

### create user

Better attach to group.

## Indexing

`Indexing` set the shared folders and type of files to be indexed.

Prefer not to convert by schedule as of time of writing there is no
de-duplication app that will take care and clean up of all lower quality copies.

## Multimedia

### install Multimedia Server package

This allows for low level and quick access to certain folders.

We can enable transcoding server side compressing dense and heavy multimedia.

### install Advanced Media Extensions package

Will allow system run certain compressed techniques.

### Audio and Video Station

Skipping

### install Synology Photos package

`Synology Photos` is searching `/photo` as indexed or `/home/Photos`.

Personal space is at `home` and shared space is the shared folder. It needs be
enabled per user bases or they wont see it using the app.

As of time of writing *unless the options are enabled* the AI won't work:

- Enable the People album in Personal Space
- Enable the subject shared space...

IMPORTANT: as of time of writing Synology is not using external service.

Going trough the faces we can rename the avatars and merge unknown people.

We can download photos from google but without the metadata.

## Shared folders

### enable datachecksum for advanced technology

Do for *every* shared folder.

### enable recycle bin

But do not forget to schedule the empting of those.

## General

### enable power recovery

`Control Panel -> Hardware & Power -> General -> Power Recovery`

Rules of thumb:

- restart automatically when power is on

### enable restart automatically when power is on

### check out beep control

### cool fan mode

### set 3 hours hdd hibernation

## HDD recovery

- [recover trough pc](https://kb.synology.com/vi-vn/DSM/tutorial/How_can_I_recover_data_from_my_DiskStation_using_a_PC)
- [more realistic tutorial](https://zarino.co.uk/post/synology-shr-raid-1-ubuntu/)

The gist:
```
root@pop-os:~# mdadm -AsfR && vgchange -ay
mdadm: No arrays found in config file or automatically

lsblk
# md0 is aritrary; sdb5 is the lsblk one holing logical volume
mdadm --assemble --run /dev/md0 /dev/sdb5 --force
# whe busy: mdadm --stop --scan
lvscan
vgchange -ay vg1000 # activiate volume
mount /dev/vg1000/lv /mnt/hd1 -o ro
# when finish
umount /mnt/hd1
vgchange -an vg1000
mdadm --stop /dev/md0
```

## References

- [Simple Synology Settings EVERYONE should be using (Basics) by SpaceRex](https://www.youtube.com/watch?v=GL11Tq_W6FE)
- [Settings EVERY Synology NAS should have in 2024 by SpaceRex](https://www.youtube.com/watch?v=DQUFEs1OzRs)
- [Synology NAS Compete Build, Setup, RAID, Pools and Volumes (2024 SETUP GUIDE <SERIES>) by NASCOMPARE](https://www.youtube.com/watch?v=TDV6uCH-4Ic)
- [How to Setup a Synology NAS for the first time in DSM 7 (Complete Guide for 2021+) by SpaceRex](https://www.youtube.com/watch?v=oWujGFVATiI)
- [COMPLETE BEGINNER’S GUIDE for Synology NAS - 2023 DSM 7.2 by SpaceRex](https://www.youtube.com/watch?v=T1xW97eyXB8)
- [Synology NAS Setup & Configuration Guide! by WunderTech](https://www.wundertech.net/synology-nas-initial-setup-ultimate-guide/)
