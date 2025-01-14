The Atomic GUI aids in the creation of new atomics by providing a web form that can be filled out in order to generate the YAML test definition. This YAML can then by copy and pasted into the YAML for the appropriate Technique Number (e.g. T1003) in order to add a new atomic test. Instructions for using the Atomic GUI are provided below.

![AtomicGUIgif](https://user-images.githubusercontent.com/22311332/83468238-656db080-a439-11ea-8123-25bf45068961.gif)

## Step 1: Start the Atomic GUI
**Note**: To use the Atomic GUI you must have first [installed the execution framework](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Installing-Atomic-Red-Team) and [imported the module](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Import-the-Module).

From a PowerShell prompt where the Invoke-AtomicRedTeam module is imported, run the following command to start the Atomic GUI.

```powershell
Start-AtomicGUI
```

This will start the Atomic GUI on port 8487 by default and launch a Web Browser with the GUI loaded. The web server is bound to the localhost interface only, it is not accessible from another computer on the network. You can specify a different port number to use with the `-Port <portNum>` parameter. Note: that the copy/paste functionality **does not work well from Edge**, please use another web browser (just visit the localhost:8487 in another browser after starting the GUI).

The first time you start the GUI, the PowerShell Universal Dashboard module will be installed if needed.

You can stop the server from running with the `Stop-AtomicGUI` command

## Steps 2 & 3: Fill In the Form and Generate the YAML

Use the provided web form to define the new atomic. Any optional components can be left blank if you don't intend to use them. Once you've filled out the form, click the `Generate Test Definition YAML` button.

![image](https://user-images.githubusercontent.com/22311332/83467703-f6438c80-a437-11ea-8747-3152ce15b35c.png)

## Steps 4: Adjust Indentation

YAML uses the indentation level of each line of text to determent which elements belong to which parent elements. Therefore it is important that the indentation of your YAML match the indentation of the parent YAML file you will be adding your generated YAML to.

Use the left and right arrow buttons to increase and decrease the indentation as needed.

![image](https://user-images.githubusercontent.com/22311332/83550142-998db380-a4c3-11ea-86ee-dbf726008587.png)

## Steps 5 & 6: Copy YAML & Paste into Parent Technique YAML File

Use the "Copy" button to copy and then paste the generated YAML into the parent YAML file of an exiting technique (e.g. T1564.004.yaml). Make sure the indention of the YAML for your new atomic matches that of other atomics within the parent YAML file.