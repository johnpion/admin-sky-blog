# VirtualUSB

## Installation
```shell
mkdir -p /opt/usb
cd /opt/usb
wget http://www.virtualhere.com/sites/default/files/usbserver/vhusbdx86_64
chmod +x vhusbdx86_64
```

## Autostart
Add the following to **/etc/rc.local**:
```shell
cd /opt/usb/ && ./vhusbdx86_64 &
```

## Device IDs for Token Manufacturers
| Manufacturer | Vendor ID | Product ID | Notes |
|--------------|-----------|------------|-------|
| Jacarta      | 24dc      | 0101       | Automatic detection may not work due to the serial number being 00000000 |
