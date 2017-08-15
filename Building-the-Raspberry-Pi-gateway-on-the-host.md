This documents how to build the raspberry pi gateway on the host.

# Create a docker image

I'm not sure if all of the following is required:
```
sudo apt install docker.io apparmor apparmor-utils qemu-user-static wget
```

# Create a wrapper

```
sudo docker run sdthirlwall/raspberry-pi-cross-compiler > ~/bin/rpxc
chmod +x ~/bin/rpxc
```

# Install some prerequisites

```
sudo $(type -p rpxc) apt update
sudo $(type -p rpxc) apt upgrade

rpxc install-raspbian libudev-dev
rpxc install-debian pkg-config
rpxc install-debian python
rpxc install-debian python2.7
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
```
You should be in the `docker-moziot` directory.
```
rpxc bash -c ./gateway/image/build-gateway.sh
```
This will create `openzwave.tar.gz` and `node_modules.tar.gz`