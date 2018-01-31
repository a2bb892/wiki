# Download and unpack Raspbian Stretch Lite
```
wget https://downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2017-12-01/2017-11-29-raspbian-stretch-lite.zip
```

# Run the make-prep.sh script

In the gateway repository, run the make-prep.sh script found in the images directory. The --help option will show all of the available options:
```make-prep.sh [OPTION] img-file prep-file

where OPTION can be one of:

  --noconsole     Don't enable the serial console
  --ssh           Enable ssh
  --ssid          Specify the SSID for Wifi access
  --password      Specify the password for wifi access
  --hostname      Specify the hostname
  --dd DEV        Issue a dd command to copy the image to an sdcard
  --summary       Print summary of changed files
  -h, --help      Print this help
  -v, --verbose   Turn on some verbose reporting
  -x              Does a 'set -x'
```
A typical invocation might look like:
```
make-prep.sh --dd /dev/mmcblk0 2017-11-29-raspbian-stretch-lite.zip gateway-prep-0.3.0.img
```

# Eject the SD card from the host

Make sure that you do a proper eject of the sdcard and wait for unwritten data to be written. The root partition is EXT4 and just pulling the sdcard will almost certainly corrupt it.

# Boot a Raspberry Pi using the SDCard

**IMPORTANT** If you want the final image to be used on Raspberry Pi earlier than a an RPi 3 then the base image will need to be built/booted on an earlier model of Raspberry Pi.

Once it's booted, you should be able to login through the serial console or via ssh (if you enabled ssh):
```
ssh pi@gateway.local
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

See [Uploading a base image to AWS](https://github.com/mozilla-iot/wiki/wiki/Uploading-a-base-image-to-AWS) if you'd like to upload the new base image to AWS.