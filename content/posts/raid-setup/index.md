---
title: "Setting up RAID1 array & migrating data."
date: 2024-02-06T13:56:44+07:00
# weight: 1
# aliases: ["/first"]
tags: ["data","raid"]
author: "dinhcap"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false ############CHANGE
hidemeta: false
comments: true
description: "Setting up RAID1 in Linux for increased data reliability."
#canonicalURL: "https://canonical.url/to/page/"
#disableShare: false
disableHLJS: true
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: false
UseHugoToc: false
cover:
    image: "stock/hdd.jpg" # image path/url
    alt: "Setting up HDD RAID array." # alt text
    #caption: "<text>" # display caption under cover
    relative: true # when using page bundles set this to true
params:
  cover:
    responsiveImages: false
editPost:
    URL: "https://github.com/dng-nguyn/dinhcap-dev/tree/main/content"
    Text: " Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---
I currently have two HDD drives of identical sizes (not identical in form factor and brand though), and since they are not used fully (<50% storage remaining for both), I feel like it's good to setup a RAID1 array for increased read speed and data reliability.

Two HDDs were used, and I'm on EndeavourOS Galileo for this.
## Transferring data & backing up.
First, I move my data from the WD HDD to the other HDD using `rsync`:
```bash
sudo rsync -av /hdd1/ /hdd2/hdd1/
```
This rsync command with `-a` option is commonly used for backup purposes, which is what we want here. It keeps all data intact, including permissions, ownerships, timestamps, etc.. to make sure that the data is identical.

If you're not sure if your data is fully transferred or not, you can redo the `rsync` command, with the `-c` option (checksum checking) to make sure that the data is intact. `rsync` will compare the files and if some files are corrupted, it will redo the transferring again:
```bash
sudo rsync -avc /hdd1/ /hdd2/hdd1/
```
Be warned that this will take a longer time, because it has to read all the files again, while also comparing checksums. On my HDD, it took around ~30 minutes for 50GB of data, which was substantially slower than the initial transfer, but your mileage may vary.

At the time of writing and setting this up, I wasn't really sure whether setting up a RAID1 array with `mdadm` would actually delete my files on the drive or not. I couldn't really find a definite answer, and the two drives contained my backup data, so I did not risk it and backed up the hdd2 drive to my root SSD. So it's time to use the `rsync` command before - again.
```bash
sudo rsync -a --info=progress2 /hdd2/ /ssd/hdd2/
```
Since I'm moving two drives worth of data here, I used `--info=progress2` to see the current progress and transfer speed.

The initial transfer seems to be quick and fast (80% in 10 minutes!), but after redoing the math, i realized that wasn't the case. As rsync scans for more files and verify them (this can be seen with `ir-chk` value in the rsync console), the percentage in the progress is going to decrease to reflect the real value. [A popular answer from Stack Exchange](https://unix.stackexchange.com/a/261139) explains this pretty well.

As the transfer is going to take nearly an hour, it's time to do our posture check, glass of water and do the 20-20-20 rule for your eyes.

After roughly half an hour the transfer succeeded:
```bash
217.549.770.989 99% 103,44MB/s 0:33:25 (xfr#10283, to-chk=0/11070)
```
Now it's time to move on the next step: Setting up the RAID array!

## Creating the RAID array.

These are plenty of guides when it comes to creating a RAID1 array, and I'm using two guides: One from the [Linux Raid Wiki](https://web.archive.org/web/20240205122020/https://raid.wiki.kernel.org/index.php/RAID_setup) and one from the [ArchWiki](https://web.archive.org/web/20240205122114/https://wiki.archlinux.org/title/RAID).

First, Install the software RAID utility `mdadm`:
```bash
sudo pacman -S mdadm
```
Then, go into the root account:
```bash
sudo su
```
From here, we need to partition the drives. For the sake of simplicity, I'm going with GParted, mostly because I'm already using a DE and it makes thing much more easy to see. You can use the partitioner of your choice.

My HDDs are /dev/sda and /dev/sdc, and one is formatted in xfs and one is ext4. I prefer xfs, so I formatted the other ext4 partition via the GParted utility.

![Successful partition!](./success.png#center)
{{ < image "success.png" > }}

*Note: I later found out that this was not needed anyways, since all data is formatted after the RAID setup.*

Now it's time to create the RAID1 array:
```bash
mdadm --create --verbose --level=1 --metadata=1.2 --raid-devices=2 /dev/md/MyRAID1Array /dev/sdb1 /dev/sdc1
```
with the `MyRAID1Array` a name of your choice.

![HDD information in GParted](./hdd-info.png#center)

Here in my case, GParted shows that both drives are successfully set up in a RAID1 array, with the mount point
being the virtual device `/dev/md127`. However, it is not accessible yet, since the drives are resyncing and restoring parity. This process can be viewed with `cat /proc/mdstat`:
```bash
[dinhcap@endeavour:~] $ cat /proc/mdstat
Personalities : [raid1]
md127 : active raid1 sdc1[1] sda1[0]
488252416 blocks super 1.2 [2/2] [UU]
[==>..................] resync = 13.7% (67335936/488252416) finish=62.7min speed=111790K/sec
bitmap: 4/4 pages [16KB], 65536KB chunk

unused devices: <none>
```
This is going to take a long time.

After more than an hour later (2 VALORANT matches),  `mdstat` reports that it is completed:
```bash
[dinhcap@endeavour:~] $ cat /proc/mdstat
Personalities : [raid1]
md127 : active raid1 sdc1[1] sda1[0]
488252416 blocks super 1.2 [2/2] [UU]
bitmap: 0/4 pages [0KB], 65536KB chunk

unused devices: <none>
```
And then the rest is basically followed like ArchWiki:

Update configuration file:
```bash
mdadm --detail --scan >> /etc/mdadm.conf
```
Assemble the array:
```bash
mdadm --assemble --scan
```
Now, with all that steps done, I can finally format my `md127` virtual device to xfs!

![Success!](./format-success.png#center)

It was late at night and I had school the day after, so I decided it's time to sleep and turned off the machine. The following day, on booting up the device, I was greeted with a failure to boot (emergency mode) and a device dependency (something, couldn't remember it) error! Though, I totally expected this as this has happened to me before.

The reason is that `/etc/fstab` is not updated, leading to the kernel trying to find a nonexistent drive from the previous setup. I only need to add an entry to the `/etc/fstab` and things should be working as normal.
```bash
[root@endeavour dinhcap]$ blkid
/dev/md127p1: UUID="e4fa4703-ecd0-4899-9a9e-0b62c6f95ee4" BLOCK_SIZE="4096" TYPE="xfs" PARTUUID="66e841e2-3ede-4712-8261-480cbd1bcfe1"
```
From here, I can note down the UUID of the device and mount it with fstab by adding an entry to `/etc/fstab`:
```bash
UUID=e4fa4703-ecd0-4899-9a9e-0b62c6f95ee4 /media/dinhcap/raidhdd xfs defaults,noatime 0 0
```
and also commenting out the two drives that were mounted from the previous setup:
```bash
# UUID=776a6cd5-5718-4b96-add2-42734cf43cbb     /media/dinhcap/25hdd    xfs    defaults,noatime        0       0
# UUID=ffdbf901-fae6-d901-60c9-f901fae6d901     /media/dinhcap/hdd      ext4    defaults,noatime        0       0
```
After rebooting, the machine boots straight into the OS with flying colors!

## Transferring the data.

After finishing setup the drive and mounting it, it's time to transfer the data back, again, with `rsync`. But first, I have to take ownership of the drive with `chown`:
```bash
sudo chown dinhcap:dinhcap -R ./raidhdd
````
with `ls -al`, I can tell if the change took place or not:
```
drwxr-xr-x 2 dinhcap dinhcap   6 00:20  6 Thg 2  raidhdd
```
Success!

Now I want to store the files to two different folders respective to the two drives directories I had before with `rsync`, and things should be done!

Since I've migrated my files in separate folder, now is the fun part: Creating symbolic links!

```bash
sudo ln -s /old/dir/ /sym/dir/
```

Symbolic links are basically shortcuts in Windows, that points the symlink to the specified directory. By doing this, I don't have to look through every single configurations of each app and change things, as it will still point into the correct folder. Backwards compatibility ftw!

## Conclusion.

This is the process that I've done to migrate my data from two HDDs and create a RAID1 array for my home server. I've done some unnecessary steps during the migration (not mentioned here), but I'm happy with how things turned out. I now have a more reliable backup to work with, eliminating single point of failure in my data drive.
