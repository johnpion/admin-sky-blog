# GPU passthrow to VM

## Environment
  * Server: HP ProLiant DL360 Gen9
  * CPU: Intel(R) Xeon(R) CPU E5-2690
  * GPU: NVIDIA Corporation GK208B [GeForce GT 710] [10de:128b]
  * OS: Debian 12 + Proxmox 8.0

## Configuration

**/etc/default/grub**
```
GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0 intel_iommu=on iommu=pt initcall_blacklist=sysfb_init"
```

**/etc/pve/qemu-server/$VM.conf**
```
...
hostpci0: 0000:82:00.0,rombar=0,x-vga=1
...
```

```shell
echo "blacklist nvidia*" >> /etc/modprobe.d/blacklist.conf 
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf 
update-grub
update-initramfs -u -k all
reboot
```

**VM**
download and install https://us.download.nvidia.com/Windows/474.64/474.64-desktop-win10-win11-64bit-international-dch-whql.exe
