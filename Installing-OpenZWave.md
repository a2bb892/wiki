# Installing OpenZWave

## Build Yourself

To make sure you have the latest version of OpenZWave we recommend you download and build the source yourself.

### Linux

First you need to install pkg-config.

On Ubuntu:
```
 $ sudo apt-get install pkg-config
```

Then download and build the OpenZWave source code:

```
$ cd
$ git clone https://github.com/OpenZWave/open-zwave.git
$ cd open-zwave
$ make && sudo make install
```

### Mac OS
Install pkg-config following the instructions [here](http://macappstore.org/pkg-config/).

Then download and build the OpenZWave source code:

```
$ cd
$ git clone https://github.com/OpenZWave/open-zwave.git
$ cd open-zwave
$ make && sudo make install
```

## Download a Binary

Alternatively you can try installing a pre-built binary for your operating system.

### Linux
The OpenZWave website hosts [binaries](http://old.openzwave.com/downloads/) for various Linux distributions (be sure to install both libopenzwave and libopenzwave-dev).

### Mac OS

On Mac OS you can try installing OpenZWave using [HomeBrew](https://brew.sh/). 

```
$ brew install open-zwave
```

**Note:** Using the MozIoT gateway didn't work with this method last time we tried it.