This page documents what's included in the Raspberry Pi image and how it gets built.

# Initial Image

We start with an image containing Raspbian Lite. We typically use whatever the latest release is.

# Basic Preparation

The basic image gets augmented slightly by running the [`make-prep.sh`](https://github.com/mozilla-iot/gateway/blob/master/image/make-prep.sh) script. This script enables the serial console, enables I2C, sets the hostname to `gateway`, and copies [prepare-base.sh](https://github.com/mozilla-iot/gateway/blob/master/image/prepare-base.sh) to the `pi` user's home directory.

# Creation of the base image

The base image is then created by booting the above image and running `prepare-base.sh`.

# `prepare-base.sh`

`prepare-base.sh` installs dependencies, creates some `systemd` unit files (used to start the gateway, manage updates, and start the intent parser). It also sets up a script to cause `iptables` to redirect port 80 to port 8080 and port 443 to port 4443. These redirections allow the gateway to run as a non-root user.

# Build the gateway itself

The [`rpi-image-builder`](https://github.com/mozilla-iot/rpi-image-builder) repository contains the `build-image.sh` script which is used in conjunction with the `dhylands/raspberry-pi-cross-compiler-stretch` docker image to cross compile native dependencies. The actual build is performed using GitHub Actions.

# Create Final Image

The [`add-gateway.sh`](https://github.com/mozilla-iot/gateway/blob/master/image/add-gateway.sh) script is then used to add the gateway into the base image to create the final image.