## Getting Started

Before you can use the **_Invoke-AtomicTest_** function, you must first import the module:

```powershell
Import-Module C:\AtomicRedTeam\execution-frameworks\Invoke-AtomicRedTeam\Invoke-AtomicRedTeam\Invoke-AtomicRedTeam.psm1
```

Note: Your path to the **_Invoke-AtomicRedTeam.psm1_** may be different.


#### Display Test Details for the given Technique Number without Executing any Tests

```powershell
Invoke-AtomicTest T1089 -ShowDetails
```

Using the `ShowDetails` switch causes the test details to be printed to the screen and allows for easy copy and paste execution.
Note: you may need to change the path where the test definitions are found with the `PathToAtomicsFolder` parameter.

#### Display Only Test Names and Numbers

```powershell
Invoke-AtomicTest All -ShowDetailsBrief
```

#### Execute All Attacks for a Given Technique

```powershell
Invoke-AtomicTest T1117
```

This assumes your atomics folder is in the default location of `<BASEPATH>\AtomicRedTeam\atomics`

Where `<BASEPATH>` is `C:` in Windows or `~` in Linux/MacOS

You can override the default path to the atomics folder using the `$PSDefaultParameterValues` preference variable as shown below.

```
$PSDefaultParameterValues = @{"Invoke-AtomicTest:PathToAtomicsFolder"="C:\Users\myuser\Documents\code\atomic-red-team\atomics"}
```

Tip: Add this to your PowerShell profile so it is always set to your preferred default value.

#### Execute Specific Attacks (by Attack Number) for a Given Technique

```powershell
Invoke-AtomicTest T1117 -TestNumbers 1, 2
```

#### Execute Specific Attacks (by Attack Name) for a Given Technique

```powershell
Invoke-AtomicTest T1117 -TestNames "Regsvr32 remote COM scriptlet execution","Regsvr32 local DLL execution"
```

#### Speficy a Process Timeout

```powershell
Invoke-AtomicTest T1117 -TimeoutSeconds 15
```

If the attack commands do not exit (return) within in the specified `-TimeoutSeconds`, the process and it's children will be forcefully terminated. The default value of `-TimeoutSeconds` is 120. This allows the `Invoke-AtomicTest` script to move on to the next test.

#### Execute All Tests

Execute all Atomic tests:

```powershell
Invoke-AtomicTest All
```

#### Execute All Tests from a Specific Directory

Specify a path to atomics folder, example C:\AtomicRedTeam\atomics

```powershell
Invoke-AtomicTest All -PathToAtomicsFolder C:\AtomicRedTeam\atomics
```

#### Specify an Alternate Path for the Execution Log

```powershell
Invoke-AtomicTest T1117 -ExecutionLogPath 'C:\Temp\mylog.csv'
```

By default, test execution details are written to `Invoke-AtomicTest-ExecutionLog.csv` in the current directory. Use the `-ExecutionLogPath` parameter to write to a different file. Execution is only logged in the execution log when the attack commands are run (not when `-ShowDetails` , `-CheckPrereqs`, `GetPrereqs`, or `-Cleanup` swiches are used). Use the `-NoExecutionLog` switch to not write execution details to disk.

#### Check that Prerequistes for a given test are met

```powershell
Invoke-AtomicTest T1117 -TestNumber 1 -CheckPrereqs
```

For the "command_prompt", "bash", and "sh" executors, if any of the prereq_command's return a non-zero exit code, the pre-requisites are not met. Example: **fltmc.exe filters | findstr #{sysmon_driver}**

For the "powershell" executor, the prereq_command's are run as a script block and the script must exit 0 if the pre-requisites are met. Example: **if(Test-Path C:\Windows\System32\cmd.exe) { exit 0 } else { exit 1 }**

Pre-requisites will also be reported as not met if the test is defined with `elevation_required: true` but the current context is not elevated. You can still execute an attack even if the pre-requisites are not met but execution may fail.

#### Get Prerequistes

```powershell
Invoke-AtomicTest T1117 -TestNumber 1 -GetPrereqs
```

This will run the "Get Prereq Commands" listed in the Dependencies section for the test.

The execution framework provides a helpful PowerShell function called `Invoke-WebRequestVerifyHash` which only downloads and saves a file to disk if the file hash matches the specified value. Call this method by passing in the url of the file to download, the path where it should be saved, and lastly the expected Sha256 file hash.
The function returns `$true` if the file was saved to disk, `$false` otherwise.

Important Note: You must add the import of `Invoke-WebRequestVerifyHash.ps1` or the entire `Invoke-AtomicRedTeam.psm1` to your PowerShell profile to make this function available to the prereq commands.

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