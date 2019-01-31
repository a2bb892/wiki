This page documents whats included in the Raspberry Pi image and how it gets built.

# Initial Image

We start with an image containing Raspbian. We typically use whatever the latest release is.

# Basic Preperation

The basic image gets augmented slightly by running the https://github.com/mozilla-iot/gateway/blob/master/image/make-prep.sh script. This script enables the serial console, enables I2C, sets the hostname to `gateway`, and copies [prepare-base.sh](https://github.com/mozilla-iot/gateway/blob/master/image/prepare-base.sh) and [prepare-base-root.sh](https://github.com/mozilla-iot/gateway/blob/master/image/prepare-base-root.sh) to the pi user's home directory.

# Creation of the base image

The base image is then created by booting the above image and running prepare-base.sh

# prepare-base.sh

prepare-base.sh installs dependencies, creates some systemd control files (used to start the gateway, manage updates, renew lets encrypt certificates, and start the intent parser). It also sets up a script to cause iptables to redirect port 80 to port 8080 and port 443 to port 4443. These redirections allow the gateway to run as a non-root user.

# prepare-base-root.sh

prepare-base-root.sh should be merged with prepare-base.sh and exists primarily for historical reasons. It currently sets up hostapd and dnsmasq which are used as part of the first time setup.

# Build the gateway itself

The [rpi-image-builder](https://github.com/mozilla-iot/rpi-image-builder) repository contains the build-image.sh script which is used in conjunction with the dhylands/raspberry-pi-cross-compiler-stretch docker image to cross compile native dependencies. These include the openzwave library, and any native node dependencies used by the gateway. The actual build is performed using travis.

# Create Final Image

The [add-gateway.sh](https://github.com/mozilla-iot/gateway/blob/master/image/add-gateway.sh) script is them used to add the gateway and openzwave into the base image to create the final image.
