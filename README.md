# klipper-config

After installing KIAUH, download the config folder:
```
cd ~/printer_data/
mv config config_orig
git clone git@github.com:thiagosalles/klipper-config.git config
cp -r config_orig/. config/
rm -rf config_orig/
cd config/ && git checkout printer.cfg
```

## Reference

- https://github.com/Klipper3d/klipper/blob/master/config/sample-macros.cfg
- https://www.klipper3d.org/Measuring_Resonances.html

## SD Card Troubleshooting

It has happened before that the SD card gets corrupted.

To fix this, first remove the card from the 3D printer and insert it into a machine with Linux. Then, execute the following command to list the partitions:
```
sudo fdisk -l
```

You should see something like this:
```
Disk /dev/sda: 29.28 GiB, 31440502784 bytes, 61407232 sectors
Disk model: Mass-Storage
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x8a5298e3

Device     Boot  Start      End  Sectors  Size Id Type
/dev/sda1         8192   532479   524288  256M  c W95 FAT32 (LBA)
/dev/sda2       532480 61407231 60874752   29G 83 Linux
```

All you need to do is use the `fsck` command on each of the partitions to recover them. Use the following commands:
```
sudo fsck -fy /dev/sda1
sudo fsck -fy /dev/sda2
```

The output should be something like this:
```
fsck from util-linux 2.36.1
fsck.fat 4.2 (2021-01-31)
There are differences between boot sector and its backup.
This is mostly harmless. Differences: (offset:original/backup)
  65:01/00
  Not automatically fixing this.
Dirty bit is set. Fs was not properly unmounted and some data may be corrupt.
 Automatically removing dirty bit.

*** Filesystem was changed ***
Writing changes.
/dev/sda1: 502 files, 26801/130554 clusters
```
and this:
```
fsck from util-linux 2.36.1
e2fsck 1.46.2 (28-Feb-2021)
rootfs: recovering journal
Journal transaction 66068 was corrupt, replay was aborted.
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
Free blocks count wrong (5991157, counted=5991152).
Fix? yes

Free inodes count wrong (1748914, counted=1748904).
Fix? yes


rootfs: ***** FILE SYSTEM WAS MODIFIED *****
rootfs: 100184/1849088 files (0.3% non-contiguous), 1618192/7609344 blocks
```

If it happens again, suspect the SD card you are using. It is recommended to replace it in this case.

Use the following command to generate a backup image of your original (and repaired) SD card:
```
sudo dd if=/dev/sda of=~/klipper.img
```

Depending on the SD card you are using, it may take a long time. Please wait patiently.

After finished, insert the new SD card. Make sure it's unmounted and use the following command to write the backup image to it:

```
sudo dd if=~/klipper.img of=/dev/sdb
```

If you have any trouble cloning the SD card this way, you can try balenaEtcher software

## Input Shapper and Pressure Advance

First, install the input shapper hardware on the machine and comment out the line of code that contains the `pressure_advance` in printer.cfg and uncomment the line that inclues the `adxl.cfg`

Now, run the following command
```
SHAPER_CALIBRATE
```

If it fails, just run the command again.

After all the calculations, just click on `SAVE CONFIG` button.

Open `printer.cfg` again, comment out the line that includes the `adxl.cfg` and uncomment the `pressure_advance`

