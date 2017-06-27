# Download and unpack Raspbian Jessie Lite
```
wget https://downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2017-04-10/2017-04-10-raspbian-jessie-lite.zip
unzip 2017-04-10-raspbian-jessie-lite.zip
```
Use a small SD card (2 Gb) to keep the image file size small. Unmount (don't eject) any volumes on the sdcard. If this is a new blank card then there will most likely only be a single partition. If this SD card has previously been formatted for the RPi, then there will be 2 partitions.
```
sudo umount /dev/sdh1
sudo umount /dev/sdh2
sudo dd status=progress bs=10M if=2017-04-10-raspbian-jessie-lite.img of=/dev/sdh
```
Note that the of= specifies the block device for the entire device (i.e. /dev/sdh versus /dev/sdh1 which refers to a particular partition).

# Do host prep of the image

Unplug and replug the SD card so that the newly created partitions get mounted. Under ubuntu, these will typically show up under `/media/USERNAME/boot` and `/media/USERNAME/UUID-of-root-partition`.

## Enable ssh

Create an empty file called ssh in the root of the boot partition. When the RPi boots up it will see this and enable ssh.
```
touch /media/USERNAME/boot/ssh
```

## Copy installation script to the RPi

```
cd /media/USERNAME/f2100b2f-ed84-4647-b5ae-089280112716/home/pi
sudo wget https://raw.githubusercontent.com/mozilla-iot/gateway/master/install.sh
sudo chmod +x install.sh
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

# Eject the SD card from the host and put it in the RPi and power on

Once it's booted, you should be able to do:
```
ssh pi@raspberrypi.local
```
and supply a password of raspberry. Note: if you're doing this several times, you'll get ssh errors about man-in-the-middle attacks because the RPi will often get the same IP address but will have a different host key.

# Run the install script

```
./install.sh
```
Go and get a coffee. If everything goes well, you should be able to tun app.js.

