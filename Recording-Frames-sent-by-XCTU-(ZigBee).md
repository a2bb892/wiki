The XCTU utility has the ability to record the frames sent from the XCTU utility to the devices.

* Launch XCTU
* Click on "Add Radio Module" and select the appropriate UART
* In the top right, click on the Computer Display icon "Switch to Consoles working mode"
* Click on the "Open" icon.
* Click on the "Record" icon and select where you want the log file to go.
* Click on "Detach" if you'd like the frame log window to be detached from the main window.
* Go and do things in the XCTU program and the various frames sent & received will be recorded in the log file.
* When you're done, switch back to the Consoles working mode and click on "Stop"
* You can use https://github.com/moziot/xctu-logdump or xctu to look at the log file.