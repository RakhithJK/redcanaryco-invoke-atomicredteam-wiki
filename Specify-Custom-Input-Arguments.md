## Specify Custom Input Arguments Interactively

Use the `-PromptForInputArgs` switch to set your own values for the input arguments used by the atomic test.

```powershell
Invoke-AtomicTest T1564.004 -TestNames "Create ADS command prompt" -PromptForInputArgs
```

## Scriptable Custom Input Arguments 

You can specify all, or a subset of the input parameters via the command line or a script. Any input parameters not explicitly defined will maintain their default values from the test definition yaml.

```powershell
$myArgs = @{ "file_name" = "c:\Temp\myfile.txt"; "ads_filename" = "C:\Temp\ads-file.txt"  }
Invoke-AtomicTest T1158 -TestNames "Create ADS command prompt" -InputArgs $myArgs
```

