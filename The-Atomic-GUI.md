The Atomic GUI aids in the creation of new atomics by providing a web form that can be filled out in order to generate the YAML test definition. This YAML can then by copy and pasted into the YAML for the appropriate Technique Number (e.g. T1003) in order to add a new atomic test. Instructions for using the Atomic GUI are provided below.

# Start the Atomic GUI

```powershell
Start-AtomicGUI
```

This will start the Atomic GUI on port 8487 by default and launch a Web Browser with the GUI loaded. The web server is bound to the localhost interface only, it is not accessible from another computer on the network. You can specify a different port number to use with the `-Port <portNum>` parameter.

The first time you start the GUI, the PowerShell Universal Dashboard will be installed if needed.

You can stop the server from running with the `Stop-AtomicGUI` command
