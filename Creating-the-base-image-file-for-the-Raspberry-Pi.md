# Download and unpack Raspbian Stretch Lite
```
wget http://director.downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2018-11-15/2018-11-13-raspbian-stretch-lite.zip
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
make-prep.sh --dd /dev/sdd 2018-11-13-raspbian-stretch-lite.zip gateway-0.7.0-prep.img
```

# Eject the SD card from the host

Make sure that you do a proper eject of the sdcard and wait for unwritten data to be written. The root partition is EXT4 and just pulling the sdcard will almost certainly corrupt it.

# Boot a Raspberry Pi using the SDCard

**IMPORTANT** If you want the final image to be used on Raspberry Pi earlier than a an RPi 3 then the base image will need to be built/booted on an earlier model of Raspberry Pi.

**IMPORTANT** If you want the final image to include wireless support, then your Raspberry Pi that you build the base image on should have a wifi dongle that is supported by the default raspbian image.

In light of the above two items, the images that are released for the Mozilla IoT gateway have their base images created using a Raspberry Pi 2 which has a compatible USB wireless dongle plugged in. By compatible, I mean supported by raspbian without having to install any additional wireless drivers.

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
# Remove the sdcard and make a smaller image

Remove the sdcard from the pi and insert it into the host. The first time that the image was booted it will have resized the filesystem on the sdcard to fill it. You can run the following `shrink.fs` script (found in the same directory as make-prep.sh).

```
shrink.sh /dev/sdd gateway-0.7.0-base.img
```

You can also specify an optional filesystem size (in megabytes). If not provided then it defaults to 2400 megabytes which is suitable for the headless image (i.e. stretch-lite).

# Copy the image to AWS

Run image-to-aws.sh (found in the same directory as make-prep.sh):
```
image-to-aws.sh gateway-0.7.0-base.img
```

See [this page](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) for instructions on installing the AWS command line tool. You can test that the aws command line tool is working properly by using the command:
```
aws s3 ls s3://mozillagatewayimages/base/
```
