## XenServer 6.5. Install

```shell
echo 'alias nano="nano -w"' >> ~/.bashrc
```

❗️ By default, the root partition fills up quickly with the standard disk layout.

The installation is done in several stages:
  * Boot into a clean system with sda1, prepare md0 - sdb1.
  * Boot from sdb1, add sda1 to md0.
  * Verify that the system boots from md0.
  * Create md1 and md2 (check if they are accessible after reboot).
  * Create LVM partitions for:
    * /var/log ~4G;
    * /tmp ~4G;
    * /opt/iso - ISO repository, size as needed, e.g., 50G;
  * Verify that all partitions are mounted after a reboot, and the ISO repository is accessible in XenCenter.

### mdadm
Erase the partition table on the second disk. Install a GPT partition table with a 10G partition. Set boot flags:
```shell
sgdisk /dev/sdb --zap-all --mbrtogpt --clear --zero-superblock
sgdisk /dev/sdb --new=1:2048:20973567 --attributes=1:set:2 --typecode=1:fd00
sgdisk /dev/sdb --new=2:20973568:41945087 --typecode=2:fd00
sgdisk /dev/sdb --new=3:41945088:`sgdisk -p /dev/sdb | grep last | awk {'print $10'}` --typecode=3:fd00
```

Load the raid module for mdadm to work:
```shell
# modprobe raid1
```

To make mdadm load at startup, create an executable file specifying the required modules:
**/etc/sysconfig/modules/raid.modules**
```shell
echo 'modprobe raid1' > /etc/sysconfig/modules/raid.modules
chmod +x /etc/sysconfig/modules/raid.modules
```

Create RAID arrays:
```shell
mdadm --stop /dev/md0
mdadm --create /dev/md0 --metadata=0.90 --level=1 --raid-devices=2 missing /dev/sdb1
```

Save md0 information:
```shell
mdadm --examine --scan > /etc/mdadm.conf
```

Create and mount the file system. Copy the working system to the created array:
```shell
mkfs.ext3 /dev/md0
mount /dev/md0 /mnt
cp -vax / /mnt
```

Replace the root filesystem name with /dev/md0 in the /etc/fstab file:
```shell
sed -i 's/LABEL\=root\-[a-zA-Z\-]*/\/dev\/md0/g' /etc/fstab
```

Copy the bootloader to sdb:
```shell
mount --bind /dev /mnt/dev
mount --bind /sys /mnt/sys
mount --bind /proc /mnt/proc
chroot /mnt
/sbin/extlinux --raid --install /boot
exit
dd if=/mnt/usr/share/syslinux/gptmbr.bin of=/dev/sdb
```

### initrd
Rebuild initrd:
```shell
cd /mnt/boot
mkinitrd -v --fstab=/mnt/etc/fstab initrd-`uname -r`-raid.img `uname -r`
ln -sf initrd-`uname -r`-raid.img initrd-3.10-xen.img
```

Edit **/mnt/boot/extlinux.conf** to boot from md0:
```shell
sed -i 's/LABEL\=root\-[a-zA-Z\-]*/\/dev\/md0/g' extlinux.conf
```
