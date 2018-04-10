# Installing OpenZWave

## Build Yourself

To make sure you have the latest version of OpenZWave we recommend you download and build the source yourself.

### Note about -D_GLIBCXX_USE_CXX11_ABI

Starting in g++ 5.1, there has been a change with how some of the linkage symbols work. If you're using a newer compiler, you can use -D_GLIBCXX_USE_CXX11_ABI=0 to get the old behaviour.  The addon-builder runs on Travis and uses version 4.8 of g++, so it builds libraries with the old symbols. In order to be compatible, you should build your copy of openzwave using this define as well. The instructions below show how to build with _GLIBCXX_USE_CXX11_ABI=0

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
$ CFLAGS=-D_GLIBCXX_USE_CXX11_ABI=0 mak && sudo CFLAGS=-D_GLIBCXX_USE_CXX11_ABI=0 make install
```
I found that this installed libopenzwave.so.1.4 into `/usr/local/lib64` which isn't in the default search location for shared libraies. The [node-openzwave-shared page](https://github.com/OpenZWave/node-openzwave-shared) recommendation is to put a symlink into `/usr/local/lib` and that seems reasonable to me. I also had to run ldconfig in order to update the cache.
```
$ sudo ln -s /usr/local/lib64/libopenzwave.so /usr/local/lib/libopenzwave.so
$ sudo ln -s /usr/local/lib64/libopenzwave.so.1.4 /usr/local/lib/libopenzwave.so.1.4
$ sudo ldconfig
```

### Mac OS
Install pkg-config following the instructions [here](http://macappstore.org/pkg-config/).

Then download and build the OpenZWave source code:

```
$ cd
$ git clone https://github.com/OpenZWave/open-zwave.git
$ cd open-zwave
$ CFLAGS=-D_GLIBCXX_USE_CXX11_ABI=0 make && sudo CFLAGS=-D_GLIBCXX_USE_CXX11_ABI=0 make install
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