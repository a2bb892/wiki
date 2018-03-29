# Create the base image

Follow the directions [[here|Creating-the-base-image-file-for-the-Raspberry-Pi]] to create a base image.

# Build the Raspberry Pi version of the gateway

Follow the directions [[here|Building-the-Raspberry-Pi-gateway-on-the-host]] to use docker to create a Raspberry Pi build of the gateway on the host.

# Add the Raspberry Pi gateway to the base image

## Prerequisites

The `add-gateway.sh` makes use of the kpartx and unzip tools in order to perform loop mounts of the image file. Under Ubuntu you can install this by using:
```
sudo apt install kpartx zip
```

## Run add-gateway.sh

Finally, run the following command from within the docker-moziot directory:
```
./gateway/image/add-gateway.sh -o openzwave.tar.gz -g gateway.tar.gz 2017-04-10-gateway-base.zip
```
This will create a file called: `2017-04-10-gateway-base-final.img` which can then be flashed to an sdcard using dd.