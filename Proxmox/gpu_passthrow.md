# GPU passthrow to VM

## Environment
  * Server: HP ProLiant DL360 Gen9
  * CPU: Intel(R) Xeon(R) CPU E5-2690
  * GPU: NVIDIA Corporation GK208B [GeForce GT 710] [10de:128b]
  * OS: Debian 12 + Proxmox 8.0

## Configuration

**/etc/default/grub**
```
GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0 intel_iommu=on iommu=pt"
```

**/etc/pve/qemu-server/$VM.conf**
```
...
hostpci0: 0000:82:00,x-vga=1
...
```

```shell
update-grub
update-initramfs -u -k all
reboot
```
