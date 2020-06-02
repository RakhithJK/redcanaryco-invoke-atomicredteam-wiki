The Atomic GUI aids in the creation of new atomics by providing a web form that can be filled out in order to generate the YAML test definition. This YAML can then by copy and pasted into the YAML for the appropriate Technique Number (e.g. T1003) in order to add a new atomic test. Instructions for using the Atomic GUI are provided below.

![AtomicGUIgif](https://user-images.githubusercontent.com/22311332/83468238-656db080-a439-11ea-8123-25bf45068961.gif)

## Step 1: Start the Atomic GUI

From a PowerShell prompt where the Invoke-AtomicRedTeam module is imported, run the following command to start the Atomic GUI.

```powershell
Start-AtomicGUI
```

This will start the Atomic GUI on port 8487 by default and launch a Web Browser with the GUI loaded. The web server is bound to the localhost interface only, it is not accessible from another computer on the network. You can specify a different port number to use with the `-Port <portNum>` parameter.

The first time you start the GUI, the PowerShell Universal Dashboard will be installed if needed.

You can stop the server from running with the `Stop-AtomicGUI` command

## Steps 2 & 3: Fill Out the Form and Generate the YAML

Use the provided web form to define the new atomic. Any optional components can be left blank if you don't intend to use them. Once you've filled out the form, click the `Generate Test Definition YAML` button.

![image](https://user-images.githubusercontent.com/22311332/83467703-f6438c80-a437-11ea-8747-3152ce15b35c.png)