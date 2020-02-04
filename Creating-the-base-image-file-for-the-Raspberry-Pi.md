# Prepare the Image

The following steps should be done on a Linux build host.

## Download Raspbian Lite

```
curl -JLO https://downloads.raspberrypi.org/raspbian_lite_latest
```

## Run the `make-prep.sh` script

In the gateway repository, run the `make-prep.sh` script found in the `images` directory. The `--help` option will show all of the available options:
```
make-prep.sh [OPTION] raspbian-img-file prep-file

where OPTION can be one of:

  --noconsole       Don't enable the serial console
  --ssh             Enable ssh
  --ssid SSID       Specify the SSID for Wifi access
  --password PWD    Specify the password for wifi access
  --wifi-country CC Specify the WiFi country code to use (default: GB)
  --no-i2c          Disable I2C bus
  --hostname NAME   Specify the hostname
  --dd DEV          Issue a dd command to copy the image to an sdcard
  --summary         Print summary of changed files
  -h, --help        Print this help
  -v, --verbose     Turn on some verbose reporting
  -x                Does a 'set -x'
```
A typical invocation might look like:
```
make-prep.sh --dd /dev/sdd 2019-09-26-raspbian-buster-lite.zip gateway-0.11.0-prep.img
```

## Eject the SD card

Make sure that you do a proper eject of the SD card and wait for unwritten data to be written. The root partition is EXT4 and just pulling the SD card will almost certainly corrupt it.

# Populate the Image

The following steps are done on the Raspberry Pi, to install dependencies and such into the image.

## Plug your Raspberry Pi into a wired network and a serial console

The builds that we distribute have SSH disabled by default, so you'll need a serial console and a wired network connection. The `prepare-base.sh` script needs to be able to do `sudo apt update` and `sudo apt upgrade` in order to work properly.

## Boot a Raspberry Pi using the SD card

**IMPORTANT** If you want the final image to be used on Raspberry Pi earlier than a an RPi 3 then the base image will need to be built/booted on an earlier model of Raspberry Pi.

**IMPORTANT** If you want the final image to include wireless support, then your Raspberry Pi that you build the base image on should have a Wi-Fi dongle that is supported by the default Raspbian image.

In light of the above two items, the images that are released for the Mozilla WebThings Gateway have their base images created using a Raspberry Pi 1.2 which has a compatible USB wireless dongle plugged in. By compatible, I mean supported by Raspbian without having to install any additional wireless drivers.

Once it's booted, you should be able to log in through the serial console or via SSH (if you enabled SSH) with username `pi` and password `raspberry`.

**Note:** if you're doing this several times, you may get SSH errors about man-in-the-middle attacks because the RPi will often get the same IP address but will have a different host key.

## Run the preparation script

```
./prepare-base.sh
```

Go and get a coffee. If everything goes well, you should now have a usable base image, which the `add-gateway.sh` script can use (once you've finished the remaining steps on this page).

## Shutdown

Do a clean shutdown of the Raspberry Pi
```
sudo poweroff
```

# Finalize the Image

The following steps are done on the initial Linux build host to finalize the base image.

## Remove the SD card and make a smaller image

Remove the SD card from the Pi and insert it into the host. The first time that the image was booted it will have resized the filesystem on the SD card to fill it. You can run the following `shrink.sh` script (found in the same directory as `make-prep.sh`).

```
shrink.sh /dev/sdd gateway-0.11.0-base.img
```

You can also specify an optional filesystem size (in megabytes). If not provided, then it defaults to 3000 megabytes which is suitable for the headless image (i.e. buster-lite).

## Copy the image to AWS

Run `image-to-aws.sh` (found in the same directory as make-prep.sh):
```
image-to-aws.sh gateway-0.11.0-base.img
```

See [this page](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) for instructions on installing the AWS command line tool. You can test that the `aws` command line tool is working properly by using the command:
```
aws s3 ls s3://mozillagatewayimages/base/
```