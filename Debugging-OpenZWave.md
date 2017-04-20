# Enable OpenZWave Logging

The default configuration of the OpenZWave library will have logging enabled and the contents will go into a file called OZW_Log.txt in the directory where app.js was run.

You can also cause the debug output to be directed to stdout. One way to do this is to edit the file [adapters/zwave/zwave-adapter.js](https://github.com/moziot/gateway/blob/2580d6fd45c4d3bfdeb70bbd6e8fe79cb78f197b/adapters/zwave/zwave-adapter.js#L32) file, find the ConsoleOutput option and set it to true.

When you restart the node server, you will then see copious amounts of logging.

# Build a debug version of libopenzwave

For debugging segfaults, you'll want a debug version of the openzwave library. You can do this by doing the following:
```
git clone https://github.com/OpenZWave/open-zwave.git
cd open-zwave
BUILD=debug make && sudo BUILD=debug make install
```
Note that if you're building a debug version in a tree that has previously had a release version built, then you should do a `make clean` before building the debug version.

Now that you've got a debug version of openzwave, you should be able to run app.js under gdb and get debug information (like a backtrace).
```
gdb node
```
and then when you get to the gdb prompt:
```
(gdb) run ./app.js
```

If you then get to a segfault, use the `bt` command to get a backtrace.