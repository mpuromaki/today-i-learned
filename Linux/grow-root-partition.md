# How to grow root partition (ubuntu server, minimal)

The basic scene. You messed up with your Ubuntu Server Minimal Installation running on a virtual machine. / is full.
You extend the disk size on your hypervisor. What next?

## Check what you have

First let's check situation. What disk is full?

```sh
> df -h
Filesystem                             Size  Used Avail Use% Mounted on
...
/dev/vda2                              9.3G  9.2G     0 100% /
...
/dev/vda1                              537M  6.1M  531M   2% /boot/efi
```

/dev/vda seems to be full. Root partition is on /dev/vda2.
Has the disk /dev/vda more unallocated space available?

```sh
> fdisk -l

GPT PMBR size mismatch (20971519 != 41943039) will be corrected by write.
The backup GPT table is not on the end of the device.
Disk /dev/vda: 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 82317E4E-AD48-4CC6-94BE-8102E95D5855

Device       Start      End  Sectors  Size Type
/dev/vda1     2048  1103871  1101824  538M EFI System
/dev/vda2  1103872 20969471 19865600  9.5G Linux filesystem
```

Fdisk indicates that /dev/vda2 is not filling the entire disk. 20th million sector is the last one, but the disk has 41 million sectors available.

## Extend your volume and filesystem

Luckily, even the minimal installation has the required tools.

```sh
> sudo growpart /dev/vda 2     # <---- Note the space between vda and 2.
CHANGED: partition=2 start=1103872 old: size=19865600 end=20969472 new: size=40839135 end=41943007
> sudo resize2fs /dev/vda2
resize2fs 1.46.5 (30-Dec-2021)
Filesystem at /dev/vda2 is mounted on /; on-line resizing required
old_desc_blocks = 2, new_desc_blocks = 3
The filesystem on /dev/vda2 is now 5104891 (4k) blocks long.
> sudo reboot                  # <---- Not actually necessary, but do it anyway.
```
