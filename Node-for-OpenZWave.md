Node links dynamically to the OpenZWave library, which means that you need to build (or install) the OpenZWave library locally.

I chose to build it, so these are the steps I did. This assumes that you have a C++ build environment setup and that you've previously installed npm.
```
mkdir OpenZWave
cd OpenZWave
git clone https://github.com/OpenZWave/open-zwave.git
git clone https://github.com/OpenZWave/node-openzwave-shared.git
cd open-zwave
make
sudo make install
cd ../node-openzwave-shared
npm install .
```
I then edited test2.js and changed:
```
  "linux": '/dev/ttyUSB0',
```
to be:
```
  "linux": '/dev/ttyACM0',
```
since my ZWave dongle (AEOTEC ZStick Gen5) shows up as /dev/ttyACM0.

If your dongle already has a network setup (the dongle persists information about the ZWave network) you may want to do a factory reset of the dongle and/or devices to get everything back into a clean state.

From the node-openzwave-shared directory you can now run:
```
node
> .load test2.js
```
You'll see a whole bunch of output. Scroll back to just after the .load command and double check for errors. For a successful run, I see something like this:
```js
> .load test2.js
> var ZWave = require('./lib/openzwave-shared.js');
initialising OpenZWave addon (/home/dhylands/Dropbox/moziot/OpenZWave/node-openzwave-shared/lib/../build/Release/openzwave_shared.node)
undefined
> var os = require('os');
undefined
> var zwave = new ZWave({
...   ConsoleOutput: false
... });
Initialising OpenZWave 1.4.2476 binary addon for Node.JS.
	OpenZWave Security API is ENABLED
	ZWave device db    : /usr/local/etc/openzwave
	User settings path : /home/dhylands/Dropbox/moziot/OpenZWave/node-openzwave-shared/build/Release/../../
	Option Overrides : --ConsoleOutput false
undefined
> zwavedriverpaths = {
...   "darwin": '/dev/cu.usbmodem1411',
...   "linux": '/dev/ttyACM0',
...   "windows": '\\\\.\\COM3'
... }
{ darwin: '/dev/cu.usbmodem1411',
  linux: '/dev/ttyACM0',
  windows: '\\\\.\\COM3' }
> var nodes = [];
undefined
...
> scanning homeid=0xcc9eff28...
node1: Aeotec, ZW090 Z-Stick Gen5
node1: name="", type="Static PC Controller", location=""
node1: class 32
node1:   Basic=0
====> scan complete
```

You can now add nodes to the newwork. I had a couple of Leviton Appliance modules. From the node prompt started above issue the command:
```
> zwave.addNode()
```
You should see something like this output:
```
> zwave.addNode()
true
> controller commmand feedback: ControllerCommand - Starting node==0, retval=1, state=0
controller commmand feedback: ControllerCommand - Waiting node==0, retval=4, state=0
```
Now press the pairing button on the device to add, and you should get a bunch of additional stuff printed to the console:
```
> zwave.addNode()
true
> controller commmand feedback: ControllerCommand - Starting node==0, retval=1, state=0
controller commmand feedback: ControllerCommand - Waiting node==0, retval=4, state=0
controller commmand feedback: ControllerCommand - InProgress node==0, retval=6, state=0
controller commmand feedback: ControllerCommand - InProgress node==0, retval=6, state=0
controller commmand feedback: ControllerCommand - InProgress node==0, retval=6, state=0
controller commmand feedback: ControllerCommand - Completed node==0, retval=7, state=0
node4: nop
controller commmand feedback: ControllerCommand - Starting node==0, retval=1, state=0
controller commmand feedback: ControllerCommand - InProgress node==0, retval=6, state=0
controller commmand feedback: ControllerCommand - Completed node==0, retval=7, state=0
controller commmand feedback: ControllerCommand - Starting node==0, retval=1, state=0
controller commmand feedback: ControllerCommand - InProgress node==0, retval=6, state=0
controller commmand feedback: ControllerCommand - Completed node==0, retval=7, state=0
node4: Leviton, Unknown: type=1805, id=0334
node4: name="", type="Binary Scene Switch", location=""
node4: class 37
node4:   Switch=false
node4: class 39
node4:   Switch All=On and Off Enabled
node4: class 115
node4:   Powerlevel=Normal
node4:   Timeout=0
node4:   Set Powerlevel=undefined
node4:   Test Node=0
node4:   Test Powerlevel=Normal
node4:   Frame Count=0
node4:   Test=undefined
node4:   Report=undefined
node4:   Test Status=Failed
node4:   Acked Frames=0
node4: class 134
node4:   Library Version=3
node4:   Protocol Version=3.52
node4:   Application Version=0.05
```
The variable `nodes` contains all of the nodes reported by the controller.
```
> nodes
[ ,
  { manufacturer: 'Aeotec',
    manufacturerid: '0x0086',
    product: 'ZW090 Z-Stick Gen5',
    producttype: '0x0101',
    productid: '0x005a',
    type: 'Static PC Controller',
    name: '',
    loc: '',
    classes: { '32': [Object] },
    ready: true },
  ,
  ,
  { manufacturer: 'Leviton',
    manufacturerid: '0x001d',
    product: 'Unknown: type=1805, id=0334',
    producttype: '0x1805',
    productid: '0x0334',
    type: 'Binary Scene Switch',
    name: '',
    loc: '',
    classes: 
     { '37': [Object],
       '39': [Object],
       '115': [Object],
       '134': [Object] },
    ready: true } ]
```
In the above example, the node was added as node4 so there are some empty entries in the array. If your ZWave dongle was factory reset, then the first entry will most likely be node2.

You can turn the appliance module on or off using a command like:
```
> zwave.setValue(4, 37, 1, 0, true)
undefined
> node4: changed: 37:Switch:false->true
> zwave.setValue(4, 37, 1, 0, false)
undefined
> node4: changed: 37:Switch:true->false
```
The arguments to setValue are: `zwave.setValue(nodeid, commandclass, instance, index, value);` test2.js (included with the repository) doesn't collect the values advertised from the nodes, and this is normally where most of the argument values would originate from. In my example above, I would use a nodeid of 4. The command class of 37 for the appliance switch corresponds to `COMMAND_CLASS_SWITCH_BINARY`. The instance and index of 1 and 0 are typical for single function devices.