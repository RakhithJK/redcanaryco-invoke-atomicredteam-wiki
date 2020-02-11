## Getting Started

Before executing an atomic test you should have done the following:
* [Installed Atomic Red Team](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Installing-Atomic-Red-Team)
* [Imported the Module](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Import-the-Module)
* [Checked Prerequisites](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Check-or-Get-Prerequisites-for-Atomic-Tests)

You may find it useful to [List Atomic Tests](https://github.com/redcanaryco/invoke-atomicredteam/wiki/List-Atomic-Tests) before execution as well.

#### Execute Specific Attacks (by Attack Number) for a Given Technique

```powershell
Invoke-AtomicTest T1117 -TestNumbers 1, 2
```

This assumes your atomics folder is in the default location of `<BASEPATH>\AtomicRedTeam\atomics`

Where `<BASEPATH>` is `C:` in Windows or `~` in Linux/MacOS

You can override the default path to the atomics folder using the `$PSDefaultParameterValues` preference variable as shown below.

```
$PSDefaultParameterValues = @{"Invoke-AtomicTest:PathToAtomicsFolder"="C:\Users\myuser\Documents\code\atomic-red-team\atomics"}
```

Tip: Add this to your PowerShell profile so it is always set to your preferred default value.

#### Execute Specific Attacks (by Attack Name) for a Given Technique

```powershell
Invoke-AtomicTest T1117 -TestNames "Regsvr32 remote COM scriptlet execution","Regsvr32 local DLL execution"
```

#### Execute All Attacks for a Given Technique

```powershell
Invoke-AtomicTest T1117
```

#### Speficy a Process Timeout

```powershell
Invoke-AtomicTest T1117 -TimeoutSeconds 15
```

If the attack commands do not exit (return) within in the specified `-TimeoutSeconds`, the process and it's children will be forcefully terminated. The default value of `-TimeoutSeconds` is 120. This allows the `Invoke-AtomicTest` script to move on to the next test.

#### Execute All Tests

This is not recommended but you can execute all Atomic tests in your atomics folder with the follwing:

```powershell
Invoke-AtomicTest All
```

#### Execute All Tests from a Specific Directory

Specify a custom path to your atomics folder, example C:\AtomicRedTeam\atomics

```powershell
Invoke-AtomicTest All -PathToAtomicsFolder C:\AtomicRedTeam\atomics
```

#### Specify an Alternate Path for the Execution Log

```powershell
Invoke-AtomicTest T1117 -ExecutionLogPath 'C:\Temp\mylog.csv'
```

By default, test execution details are written to `Invoke-AtomicTest-ExecutionLog.csv` in the current directory. Use the `-ExecutionLogPath` parameter to write to a different file. Execution is only logged in the execution log when the attack commands are run (not when `-ShowDetails` , `-CheckPrereqs`, `GetPrereqs`, or `-Cleanup` swiches are used). Use the `-NoExecutionLog` switch to not write execution details to disk.

#### Specify Input Parameters on the Command Line

```powershell
$myArgs = @{ "file_name" = "c:\Temp\myfile.txt"; "ads_filename" = "C:\Temp\ads-file.txt"  }
Invoke-AtomicTest T1158 -TestNames "Create ADS command prompt" -InputArgs $myArgs
```

You can specify a subset of the input parameters via the command line. Any input parameters not explicitly defined will maintain their default values from the test definition yaml.

#### Run the Cleanup Commands For the Specified Test

```powershell
Invoke-AtomicTest T1089 -TestNames "Uninstall Sysmon" -Cleanup
```

## Additional Examples

#### Confirm

To run all tests without confirming them run using the Confirm switch to false

```powershell
Invoke-AtomicTest All -Confirm:$false
```

Or you can set your `$ConfirmPreference` to 'Medium'

```powershell
$ConfirmPreference = 'Medium'
Invoke-AtomicTest All
```