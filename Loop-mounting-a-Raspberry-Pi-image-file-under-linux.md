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
## Helper scripts

In the tools directory you'll find a couple of helper scripts which automate the process of setting up the loop mounts and mounting/unmounting. [mount-img.sh](https://github.com/mozilla-iot/wiki/blob/master/tools/mount-img.sh) will create the loop mounts and mount the root parition. [umount-img.sh](https://github.com/mozilla-iot/wiki/blob/master/tools/umount-img.sh) undoes the work done by mount-img.sh.



## Manual steps required for loop mounting
### Create the loop mounts
```
sudo kpartx -v -a 2017-04-10-gateway-build.img
```

### Mount the loop
/dev/mapper/loop0p1 will correspond to the boot partition, and /dev/mapper/loop0p2 will correspond to the root parition.
```
sudo mkdir -p rpi-root
sudo mount /dev/mapper/loop0p1 rpi-root
```

### Unmount the loop when finished
```
sudo umount rpi-root
```

### Remove the loop device
```
kpartx -v -d 2017-04-10-gateway-build.img
```
