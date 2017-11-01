# Download and unpack Raspbian Jessie Lite
```
wget https://downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2017-09-08/2017-09-07-raspbian-stretch-lite.zip
```
Use a small SD card (2 Gb) to keep the image file size small. Unmount (don't eject) any volumes on the sdcard. If this is a new blank card then there will most likely only be a single partition. If this SD card has previously been formatted for the RPi, then there will be 2 partitions.
```
sudo umount /dev/sdh1
sudo umount /dev/sdh2
unzip -p 2017-09-07-raspbian-stretch-lite.zip | sudo dd status=progress bs=10M of=/dev/sdh
```
Note that the of= specifies the block device for the entire device. i.e. use `/dev/sdh` or `/dev/mmcblk0` and not `/dev/sdh1` or `/dev/mmcblk0p1`. A more technical way of saying this is ensure that when you mask the minor number of the block device you're using with 0x0F then you should get zero.

# Do host prep of the image

Unplug and replug the SD card so that the newly created partitions get mounted. Under ubuntu, these will typically show up under `/media/USERNAME/boot` and `/media/USERNAME/UUID-of-root-partition`.

## Enable ssh (optional)

Create an empty file called ssh in the root of the boot partition. When the RPi boots up it will see this and enable ssh.
```
touch /media/USERNAME/boot/ssh
```

**Note that if you do enable ssh, you should probably disable it when you're finished with the base image. We don't want to distribute images which have ssh enabled using a default username and password since its a security risk.**

## Copy base preparation script to the RPi

```
cd /media/USERNAME/f2100b2f-ed84-4647-b5ae-089280112716/home/pi
sudo wget https://raw.githubusercontent.com/mozilla-iot/gateway/master/image/prepare-base.sh
sudo chmod +x prepare-base.sh
```

## Enable serial console (optional)

Edit the config.txt file in the root of the boot partition and add the following 3 lines:
```
enable_uart=1
force_turbo=1
core_freq=250
```
Note: This will reduce the CPU frequency to 600 MHz.

## Enable Wifi access (optional)

Add the following to the end of the etc/wpasupplicant/wpasupplicant.conf file found in the root partition (not the boot partition), and add something like the following:
```
network={
    ssid="YOUR-AP-SSID"
    psk="YOUR-AP-PASSWORD"
}
```
This will allow the RPi to connect to a wireless network automatically.

## Change the hostname of the RPi (optional)

Edit the hostname found in `/media/USERNAME/f2100b2f-ed84-4647-b5ae-089280112716/etc/hostname` If you do this, then
you'll need to use that hostname followed by .local when ssh'ing into the board below.

You'll also need to edit the etc/hosts file and find the line that looks like:
```
127.0.1.1	raspberrypi
```
(it was the last line of the file in the image currently being used). Change `raspberrypi` to match the hostname in the etc/hostname file.

By default, we use a hostname of `gateway`.

# Eject the SD card from the host

Make sure that you do a proper eject of the sdcard and wait for unwritten data to be written. The root partition is EXT4 and just pulling the sdcard will almost certainly corrupt it.

# Boot a Raspberry Pi using the SDCard

**IMPORTANT** If you want the final image to be used on Raspberry Pi earlier than a an RPi 3 then the base image will need to be built on an earlier model of Raspberry Pi.

Once it's booted, you should be able to login through the serial console or via ssh:
```
ssh pi@raspberrypi.local
```
and supply a password of raspberry. Note: if you're doing this several times, you'll get ssh errors about man-in-the-middle attacks because the RPi will often get the same IP address but will have a different host key.

# Run the preparation script

```
./prepare-base.sh
```
Go and get a coffee. If everything goes well, you should now have a usable base image, which the `add-gateway.sh` script can use (once you've finished the remaining steps on this page).

# Shutdown

So a clean shutdown of the Raspberry Pi
```
sudo poweroff
```

# Remove the SDcard and make an image of it

When you stick the sdcard into the host it will most likely automatically mount both partitions, so you'll need to unmount those first (don't eject using the UI, but rather umount from the command line):
```
umount /dev/sdh1
umount /dev/sdh2
```

Replace `/dev/sdh` with the appropriate device for your sdcard. Make sure you use the block device for the entire sdcard and not a block device for one of the partitions.
```
sudo dd status=progress bs=10M of=2017-09-07-gateway-base.img if=/dev/sdh
zip 2017-09-07-gateway-base.img.zip 2017-09-07-gateway-base.img
sudo rm 2017-09-07-gateway-base.img
```
