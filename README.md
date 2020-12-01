# Home Automation
Installation and configuration of home automation components


# RPI-NAS
  Server configuration for Raspberry PI 4 NAS

TODO:
1. Port the ZFS configuration to an Ansible playbook.
2. Port the Samba configs to Ansible too.
3. Implement an ansible-pull setup to rebuild this machine.

This build is based off of Ubuntu 20.10 64-bit for Raspberry PI. 
Once the image is built, just copy the 'network-config' and 'user-data' files to the /boot partition.

## ZFS Setup
The zfsutils-linux package is installed via the user-data section.
The storage on this machine is managed within a ZFS pool called 'tank'. In the event that the server needs to
 be rebuilt, you should just need to attach the drives (currently via USB3 external enclosures) and run:
 ```bash
 sudo zpool import tank
 ```

 If you're starting from scratch on a new build then create the pool as needed. Originally it was set up as a 4-drive RAIDZ1 with the 
 following datasets using default mount points - (I'll automate this later using Ansible when I have time):
    * tank
    * tank/datafiles        -> A collection of backups and random data used in various projects
    * tank/general          -> Catch-all area for stuff that doesn't have a home yet
    * tank/library          -> Ebooks/PDFs
    * tank/movies           -> Ripped movies from DVD collection for Plex
    * tank/music            -> Ripped CDs
    * tank/photos           -> Photos
    * tank/tvshows          -> Ripped TVs shows from DVD collection

Compression is enabled on the datafiles and general folders:
```bash
    sudo zfs compression=on tank/general
    sudo zfs compression=on tank/library
```

Since this is a dedicated file server and it has 8GB of memory, I set the min/max arc memory for zfs as:
```bash
# /etc/modprobe.d/zfs.conf
options zfs zfs_arc_min=536870912
options zfs zfs_arc_max=6442450944
```


## Samba
TODO:
The Samba configuration:
1. Install package (This is handled via the user-data file currently)
2. Stop service
3. Create smbuser and smbgroup
4. Copy smb.conf and shares.conf to /etc/samba
5. Start service
6. Test shares


## Cockpit
TODO:
Cockpit installation is handled via the user-data file currently





