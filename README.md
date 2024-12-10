# Synology DiskStation Manager

Notes of best practices found around to setup a Synology NAS for home
use. References included at the end. The guide is based on `DSM-7.2`.

## Initial setup

Follow the install instructions after locating the attached NAS on the same
network using [finds.synology.com](http://find.synology.com/).

## Storage

### keep copy of old data around

HDD are most likely to die either in the first few weeks and last for very long
time most likely. *resource needed*

### make volume

You should be greeted with prompt, do as you see wish or flow with the
defaults. If missing the wizard use the `Storage manager`.

Leave the drive checking, it will take some time.

### schedule scrubbing pools

- make it every three months
- select to run it later, as it is a new install

### schedule drive tests

`HDD/SSD -> Settings`

- SMART quick test: every month
- SMART extended test: every three months

### install exFAT access package

### install Synology snapshot replication package

Setup up per shared folder. Setting retention period of 28 days would mean that
delete files will free up the space after 28 days of the deletion.

### install Hyper backup package

### schedule empty recycle bin tasks

`Control Panel -> Task Scheduler -> Create`

Preferably do it every single day for all recycle bins and safe to save the for
some amount of time, e.g. 7 days. Snapshots should be good enough.

### Prefer SMB connection

Should be work out of box.

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

## Control panel

### Fast clone

`File services -> Advanced ` and then select `Fast clone`

### Bypass traverse checking

`File services -> Advanced ` and then select `Enable bypass traverser checking`

## References

- [Synology NAS Compete Build, Setup, RAID, Pools and Volumes (2024 SETUP GUIDE <SERIES>) by NASCOMPARE](https://www.youtube.com/watch?v=TDV6uCH-4Ic)
- [How to Setup a Synology NAS for the first time in DSM 7 (Complete Guide for 2021+) by SpaceRex](https://www.youtube.com/watch?v=oWujGFVATiI)
- [COMPLETE BEGINNER’S GUIDE for Synology NAS - 2023 DSM 7.2 by SpaceRex](https://www.youtube.com/watch?v=T1xW97eyXB8)
- [Synology NAS Setup & Configuration Guide! by WunderTech](https://www.wundertech.net/synology-nas-initial-setup-ultimate-guide/)
