## XenServer 7.0. Install

[Based on this post](https://discussions.citrix.com/topic/378478-xenserver-7-raid1-mdadm-after-install-running-system/)

❗️Warning❗️

```shell
sed -i 's/LABEL=root-[a-zA-Z\-]*/\/dev\/md1/' /mnt/etc/fstab
sed -i 's/LABEL=swap-[a-zA-Z\-]*/\/dev\/sda6/' /mnt/etc/fstab
sed -i 's/LABEL=logs-[a-zA-Z\-]*/\/dev\/md5/' /mnt/etc/fstab
sed -i '/sda6/ a\/dev/sdb6          swap      swap   defaults   0  0 ' /mnt/etc/fstab

sed -i 's/LABEL=root-[a-zA-Z\-]*/\/dev\/md1/' /boot/grub/grub.cfg

xe sr-create content-type=user type=lvm device-config:device=/dev/md4 shared=false name-label="Local storage - HDD"
xe sr-create content-type=user type=lvm device-config:device=/dev/md4 shared=false name-label="Local storage - HDD"
mdadm --create /dev/md12 --level=1 --raid-devices=2 missing /dev/sdb4
xe sr-create content-type=user type=lvm device-config:device=/dev/mapper/1TB shared=false name-label="Crypt storage"
```
