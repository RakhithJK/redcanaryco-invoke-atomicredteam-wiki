You can specify all, or a subset of the input parameters via the command line. Any input parameters not explicitly defined will maintain their default values from the test definition yaml.

```powershell
$myArgs = @{ "file_name" = "c:\Temp\myfile.txt"; "ads_filename" = "C:\Temp\ads-file.txt"  }
Invoke-AtomicTest T1158 -TestNames "Create ADS command prompt" -InputArgs $myArgs
```

