Loop mounting allows you to mount an image file on a linux host.

## Install kpartx
```
sudo apt install kpartx
```

## Create an image file from an SD card
```
sudo umount /dev/sdf1
sudo umount /dev/sdf2
sudo dd status=progress if=/dev/sdf of=2017-04-10-gateway-build.img
```

## Create the loop mounts
```
sudo kpartx -v -a 2017-04-10-gateway-build.img
```

## Mount the loop
/dev/mapper/loop0p1 will correspond to the boot partition, and /dev/mapper/loop0p2 will correspond to the root parition.
```
sudo mkdir -p rpi-root
sudo mount /dev/mapper/loop0p1 rpi-root
```

## Unmount the loop when finished
```
sudo umount rpi-root
```

## Remove the loop device
```
kpartx -v -d 2017-04-10-gateway-build.img
```
