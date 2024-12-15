# TrueNAS Scale 23, autoexpand bug

I had an unfortunate experience when replacing smaller drives on one of my vdevs into larger ones.
The replacement and resilvering worked beautifully, but the available space on the vdev (and the zpool) did not increase.
I tried setting the drives offline and online, I tried ```zfs online -e <pool> <device>``` with no change at all.
And yes, TrueNAS sets the autoexpand parameter to on by default.

After three days of frustrated googling, it seems that this is a known issue in TrueNAS Scale 23. Just my luck.
Luckily it is easy fix with no risks for data-loss.

## The fix

As stated on this wonderful site: https://wiki.familybrown.org/manual-replacement  
Bug report here: https://ixsystems.atlassian.net/browse/NAS-126809  

```
TrueNAS has long had the ability to expand a pool (or vdev) by replacing each of its disks with larger ones.
Unfortunately, TrueNAS SCALE 23.10.0 and 23.10.1 have a bug that results in this procedure not working.
```

The fix itself is easy, as stated on that familybrown-site:

![0  known bug](https://github.com/user-attachments/assets/943cf326-de47-4e84-8091-c293a6376772)

### Results

So I followed the guidance. ```lsblk``` makes it easy to see that the devices have 2TB of physical space, but partition only spans ~500GB.

![1  lsblk](https://github.com/user-attachments/assets/04afd27b-ffb2-4676-8769-f0ed5ac3270c)

Which is easily corrected with this command for each device: ```parted /dev/<device> resizepart 1 100%```

![2  resizepart](https://github.com/user-attachments/assets/7f16d32a-a501-4845-8291-22a2b1764546)

FIXED! TrueNAS instantly shows the new available capacity of larger drives.
