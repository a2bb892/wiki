# Create the base image

Follow the directions [[here|Creating-the-base-image-file-for-the-Raspberry-Pi]] to create a base image.

# Build the Rapberry Pi version of the gateway

Follow the direction [[here|Building-the-Raspberry-Pi-gateway-on-the-host]] to use docker to create a Raspberry Pi build of the gatewy on the host.

# Add the Raspberry Pi gateway to the base image

Finally, run the following command from within the docket-moziot directory:
```
./gateway/image/add-gateway.sh -o openzwave.tar.gz -g gateway.tar.gz 2017-04-10-gateway-base.zip
```
This will create a file called: `2017-04-10-gateway-base-final.img` which an then be flashed to an sdcard using dd.