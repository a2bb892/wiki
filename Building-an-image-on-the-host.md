This documents my progress so far in getting raspberry pi native stuff built on the host.

# Create a docker image

I'm not sure if all of the following is required:
```
sudo apt install docker.io apparmor apparmor-utils qemu-user-static wget
```

# Create a wrapper

```
docker run sdthirlwall/raspberry-pi-cross-compiler > ~/bin/rpxc
chmod +x ~/bin/rpxc
```

# Install some prerequisites

```
rpxc install-raspbian libudev-dev
rpxc install-debian pkg-config
rpxc install-debian python
rpxc install-debian python2.7

sudo rpxc apt upgrade
sudo rpxc apt update
```

# Build stuff inside the docker image.

```
mkdir docker-moziot
cd docker-moziot
mkdir -p OpenZWave
cd OpenZWave
git clone https://github.com/OpenZWave/open-zwave.git
cd ..
git clone https://github.com/mozilla-iot/gateway
cd gateway
```
Grab the `cross-compile.sh` script from [PR #224](https://github.com/mozilla-iot/gateway/pull/224) and put it in the gateway directory.
```
cd ..
```
You should now be in the `docker-moziot` directory.
```
rpxc bash -c ./gateway/cross-compile.sh
```
This will create `gateway/openzwave.tar.gz` and `gateway/node_modules.tar.gz`

I then build a Raspberry Pi image following the directions [here](https://github.com/mozilla-iot/wiki/wiki/Creating-an-image-file-for-the-Raspberry-Pi) but modifying the install.sh to not do the npm install (and instead expanding the node_modules.tar.gz tarball created above) it failed because the sqlite native node module stuff wasn't built.

So that's as far as I got, and I need to figure out why the sqlite stuff isn't being built.