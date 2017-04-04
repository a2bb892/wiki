# Installing OpenZWave

## Build Yourself

To make sure you have the latest version of OpenZWave we recommend you download and build the source yourself.

### Linux & Mac OS

```
$ cd
$ git clone https://github.com/OpenZWave/open-zwave.git
$ cd open-zwave
$ make && sudo make install
```

## Download

Alternatively you can try installing a pre-built binary for your operating system.

### Linux
The OpenZWave website hosts [binaries](http://old.openzwave.com/downloads/) for various Linux distributions (be sure to install both libopenzwave and libopenzwave-dev).

### Mac OS

On Mac OS you can try installing OpenZWave using [HomeBrew](https://brew.sh/). 

```
$ brew install open-zwave
```

**Note:** Using the MozIoT gateway didn't work with this method last time we tried it.