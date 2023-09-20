# XenServer 7.2: Install

❗️Warning❗️
```
DISK_A=/dev/sdb
DISK_B=/dev/sda

dd if=/dev/zero of=$DISK_A bs=1M count=100

sgdisk $DISK_A --zap-all --mbrtogpt --clear --zero-superblock

sed -i 's/metadata_read_only = 1/metadata_read_only = 0/' /etc/lvm/lvm.conf

# sgdisk -R /dev/sdb /dev/sda
sgdisk -R $DISK_A $DISK_B

sgdisk $DISK_A --zap-all --mbrtogpt --clear --zero-superblock

sgdisk $DISK_A \
  --new=1:46139392:83888127 --attributes=1:set:2 --typecode=1:fd00 \
  --new=2:8390656:46139391 --typecode=2:fd00 \
  --new=3:83888128:84936703 --typecode=3:ef02 \
  --new=5:2048:8390655 --typecode=5:fd00 \
  --new=6:84936704:87033855 --typecode=6:8200

mdadm --create /dev/md1 --level=1 --raid-devices=2 --metadata=0.90 missing $DISK_A1
mdadm --create /dev/md2 --level=1 --raid-devices=2 --metadata=0.90 missing $DISK_A2
mdadm --create /dev/md5 --level=1 --raid-devices=2 --metadata=0.90 missing $DISK_A5

mkswap $DISK_A6

mkfs.ext4 /dev/md1
mkfs.ext4 /dev/md5

mount /dev/md1 /mnt
mkdir -p /mnt/var/log
mount /dev/md5 /mnt/var/log

mdadm --detail --scan >> /etc/mdadm.conf

cp -xR --preserve=all / /mnt

sed -i '/LABEL=root/d' /mnt/etc/fstab
sed -i '/LABEL=logs/d' /mnt/etc/fstab
sed -i '1i /dev/md1    /    ext4    defaults 0 1' /mnt/etc/fstab
sed -i '2i /dev/md5    /var/log    ext4    defaults 0 2' /mnt/etc/fstab
sed -i 's/LABEL=swap-[a-zA-Z\-]*/\/dev\/sda6/' /mnt/etc/fstab
sed -i '/sda6/ a\/dev/sdb6          swap      swap   defaults   0  0 ' /mnt/etc/fstab

e2label /dev/sda1 |xargs -t e2label $DISK_A1

mount --bind /dev /mnt/dev
mount --bind /sys /mnt/sys
mount --bind /proc /mnt/proc
chroot /mnt  /bin/bash

grub-install /dev/sdb

dracut --mdadmconf --fstab --add="mdraid" --filesystems "ext4" --add-drivers="raid1" --force /boot/initrd-$(uname -r).img $(uname -r) -M

sed -i 's/LABEL=root-[a-zA-Z\-]*/\/dev\/md1/' /boot/grub/grub.cfg
sed -i 's/quiet/rd.auto rd.auto=1 rhgb quiet/' /boot/grub/grub.cfg
sed -i '/search/ i\   insmod gzio' /boot/grub/grub.cfg
sed -i '/search/ i\   insmod part_msdos' /boot/grub/grub.cfg
sed -i '/search/ i\   insmod diskfilter mdraid09' /boot/grub/grub.cfg
sed -i '/search/ c\   set root=(hd0,gpt1)' /boot/grub/grub.cfg

################################
#
# PART 2
#
################################

mdadm --create /dev/md4 --level=1 --raid-devices=2 /dev/sda4 /dev/sdb4

xe sr-create content-type=user type=lvm device-config:device=/dev/mapper/MD4 shared=false name-label="CryptSR - SSD"
```
