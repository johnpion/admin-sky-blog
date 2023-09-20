# XenServer. Change boot order for VM ( HDD / ISO )

The script for booting debian VM

```shell

# usage:
#> ./vm_change_boot_src --cd $adba5724-46d9-002f-acff-3187a2e6d403
#> ./vm_change_boot_src --hdd $adba5724-46d9-002f-acff-3187a2e6d403

UUID=$2

function change_boot_src_to_cd {
  xe vm-param-set uuid=$UUID HVM-boot-policy="BIOS order"
  xe vm-param-clear uuid=$UUID param-name="PV-bootloader"
}

function change_boot_src_to_hdd {
  xe vm-param-set uuid=$UUID PV-bootloader="pygrub"
  xe vm-param-set uuid=$UUID PV-args="-- quiet console=hvc0"
  xe vm-param-clear uuid=$UUID param-name="HVM-boot-policy"
}

case "$1" in
  --cd ) change_boot_src_to_cd ;;
  --hdd ) change_boot_src_to_hdd ;;
esac

exit 0
```

Now you can choice where can VM boot
