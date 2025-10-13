# Synology DiskStation Manager

Configuration rules of thumb for Synology NAS, `DiskStation v7.2`. References included.

## Install

Locate the NAS with [finds.synology.com](http://find.synology.com/) and follow
the instructions; then create a storage pool.

## Storage

### schedule empty all recycle bins task

`Control Panel -> Task Scheduler -> Create`

- run daily
- set retention policy

### schedule SMART HDD tests

`Storage Manager -> HDD/SDD -> Settings -> Task Scheduler`

- SMART quick test every month
- SMART extended test every six months

### schedule pool scrubbing

`Storage Manager -> Storage -> Pool -> Schedule Data Scrubbing`

- every three months
- run it later, if a new install

### have data copies

- ideally schedule backups using `Hyper Backup`
- new HDDs either likely die in first few weeks or then probably last for long time
- it is not uncommon for HDDs in the same series to share same defect and lifespan

### install exFAT access package

- OS-wide supported file system
- suitable to make external HDD and `Hyper Backup` important data

### use Snapshot Replication

`Snapshot replicatoin -> Snapshots`

Configuration per shared folder.

- schedule daily, every 2 hours
- make the snapshots visible

Advanced retain policy:

| last of   | retain period |
|-----------|---------------|
| the hour  | 72 hours      |
| the day   | 7 days        |
| the week  | 4 weeks       |
| the month | 3 months      |

**notes**:
- Number of latest snapshots to keep prevents retention policy deleting all
snapshots when the system stops taking new snapshots.
- think as how far back granularity makes sense, e.g. do you care for hour base
changes of a file that is a moth old, or you care that you have it in that
state overall?
- should be enabled on every newly created shared folder
- retain policies constrain how quickly spaces gained back
- see "DSM Help" for an example

### use Hyper Backup package

Backup either selected folders or packages.

### install Storage Analyzer package

Reports include duplication files.

## Agreeable setup

### disable enhancing browser compatibility by skipping IP checking

`Control Panel -> Security -> Security -> General`

### set logout timer

`Control Panel -> Security -> Security -> Login Settings`

### enable adaptive MFA

`Control Panel -> Security -> Account`

- **enable** adaptive MFA
- advisable to **enable** 2-factor authentication

### enable account protection

`Control Panel -> Security -> Account -> Account protection`

### disable firewall

`Control Panel -> Security -> Firewall`

Advisable to leave disabled unless more complicated configuration is needed

### enable auto block

`Control Panel -> Security -> Protection -> Auto Block`

- Log attempts 10
- block for 5 minutes

### enable DoS Protection

`Control Panel -> Security -> Protection -> Auto Block`

### SMB settings

`Control Panel -> File Services -> SMB -> Advanced settings`

- **do not** use SMB1 as there is security vulnerability
- **disable** `NLTMv1`

### disable AFP

`Control Panel -> File Services -> AFP`

**disable** as Apple has deprecated it

### disable NFS

`Control Panel -> File Services -> NFS`

**disable** as it uses IP, bot user, authentication

### disable FTP

`Control Panel -> File Services -> FTP`

**disable** unless needed

### disable default admin user

`Control Panel -> User &  Group`

- **disable** the account named "admin"
- good idea is to generate a random secure password

### disable ssh

`Control Panel -> Terminal & SNMP`

Leave closed unless needed

### enable automatic updates for system

`Control Panel -> Updates & Restore -> DSM Update`

**enable** automatic install of important updates

### enable automatic updates for services

`Package Manager -> Settings`

**enable** automatic install of important updates

### enable security advisor

Will check out the sane and recommended by Synology configurations.

### permission viewer provides one in place overview

`Control Panel -> Permission viewer`

## Users and groups

### enable user home service

Creates `homes/` shared folder. Each user will have one folder under it. Each
user will have `home/` shortcut.

### create group

Easier to attach password settings to groups.

### create user

Easier attach to group.

### enable power recovery

`Control Panel -> Hardware & Power -> General -> Power Recovery`

- restart automatically when power is on

### enable resource usage history

`Resource Monitor -> Settings`

- **enable** usage history

### beep control

`Control Panel -> Hardware & Power -> General -> Beep control`

- know why the NAS beeps
- control whether you want it to beep

### cool fan mode

`Control Panel -> Hardware & Power -> General -> Fan Speed Mode`

- `Cool mode` should be fine

### HDD hibernation

`Control Panel -> Hardware & Power -> HDD hibernation`

- Constant start-stop HDD spinning will shorten the HDD life
- If the NAS is used daily uncheck the advanced hibernation
- Set around 2-3-4 hours;

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

Skipping. Likely those are in process of deprecation.

### install Synology Photos package

`Synology Photos` is searching `/photo` as indexed and any user `/home/Photos`.

Personal space is at `home` and shared space is the shared folder. It needs be
enabled per user bases or they wont see it using the app.

As of time of writing *unless the options are enabled* the AI won't work:

- Enable the People album in Personal Space
- Enable the subject shared space...

IMPORTANT: as of time of writing Synology is not using external service.

Going trough the faces we can rename the avatars and merge unknown people.

We can download photos from google but without the metadata.

## Shared folders

### enable data checksum

`Control Panel -> Shared Folder -> Create`

- **enable** using the wizard
- it can be checked only on creating the folder

### enable recycle bin

- **enable** using the wizard
- **do not forget** to schedule empty bin task

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

## Map subdomains to Docker Containers

Easily access NAS-hosted services via friendly subdomains instead of `diskstation.local:port`.

- local only; names accessible only on LAN
- memorable; replace numbers with easy-to-remember subdomains
- mixed services; works for both synology packages and docker containers

### set up local DNS

- install `Synology DNS Server` package
- create a `primary zone`, e.g. `example.lan` pointing to NAS IP
- add `A` record for `example.lan` pointing to NAS IP; (just leave name empty)
- add `CNAME` record for `*.example.lan` canonical name `example.lan`; (just put `*` in name)

#### set up resolution via router

Given that that this DNS likely will be the primary server, we want to resolve
only the local zones and forward all others to routers DNS or google default
DNS.

Go to `DNS Server -> Resolution`

- enable `Forwrad zones`
- add routers LAN IP as forwarder; likely `192.168.1.1`
- apply

NOTE: finding routers IP on Linux `ip route | grep default`

#### choosing a zone name

You can use almost any valid DNS zone name (letters, digits, hyphens), but
avoid:

- real public TLDs unless owned, e.g. `.com`, `.net`, `.org`, etc
- `.local` is reserved for mDNS/Bonjur; it resolves by default to the NAS `synologyname.local`;
  Use only if advanced.

Safer options: `.lan`, `.home`, `.internal`

### configure reverser proxy

Go to `Control panel -> application portal -> reverse proxy`

Add a rule

- **source**: `servicename.example.lan` (port 80 for HTTP or 443 for HTTPS)
- **destination**: `localhost:1234` where `1234` is the relevant port used in `NAS_IP:1234`

Add one entry for http and another for https, e.g. `servicename http` and
`servicename https`. They would differ into the protocol and port choice.

Repeat for each service.

### access

**Make sure** the device is using the NAS IP as DNS. Just `https://servicename.example.lan`
from browser.

#### verify from client

`nslookup servicename.example.lan`
`dig servicename.example.lan`

If fails check:

- client is using NAS DNS server (network settings)
- zone and records exist in DNS server
- forwarders are set if you need internet resolution

#### check http access

`curl -I https://servicename.example.lan [-k]`

`-k` ignores certificate warnings, useful when `https`

If failing check

- `Rverse Proxy` rules spelling,
- source hostname and port (https vs http with 443 and 80)
- destination points to the correct internal portal

#### clean dns cache

`sudo systemd-resolve --flush-caches`

## Local Docker Registry trough Container Manager

- get the `registry` from Docker Hub
- run it locally with
- create reverse proxy, e.g. `registry.example.lan`

To push...

Did not succeed to add my certificate, e.g.

- create local self signed certificate
- setup `registry.example.lan` to use it

Container Manager after restart failed to recognize the signing authority:

- putting the certificate into `/var/packages/ContainerManager/.../etc/cert`
- putting the certificate into `/usr/local/share/ca-certificates/extra`

That is why ssh-ed into the NAS and into `/var/packages/ContainerManager/etc/dockerd.json` added

```json
"insecure-registries": [
    "registry.example.lan"
]
```

Similar changes were done locally to publish into the registry.

## References

- [Simple Synology Settings EVERYONE should be using (Basics) by SpaceRex](https://www.youtube.com/watch?v=GL11Tq_W6FE)
- [Settings EVERY Synology NAS should have in 2024 by SpaceRex](https://www.youtube.com/watch?v=DQUFEs1OzRs)
- [EVERY Synology Feature Explained by SpaceRex](https://www.youtube.com/watch?v=3N0k6FdCtzI)
- [COMPLETE BEGINNER’S GUIDE for Synology NAS - 2023 DSM 7.2 by SpaceRex](https://www.youtube.com/watch?v=T1xW97eyXB8)
- [Synology NAS Compete Build, Setup, RAID, Pools and Volumes (2024 SETUP GUIDE <SERIES>) by NASCOMPARE](https://www.youtube.com/watch?v=TDV6uCH-4Ic)
- [How to Setup a Synology NAS for the first time in DSM 7 (Complete Guide for 2021+) by SpaceRex](https://www.youtube.com/watch?v=oWujGFVATiI)
- [Synology NAS Setup & Configuration Guide! by WunderTech](https://www.wundertech.net/synology-nas-initial-setup-ultimate-guide/)
