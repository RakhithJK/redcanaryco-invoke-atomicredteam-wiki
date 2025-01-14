You can use the `Invoke-AtomicTest` function to run an atomic test on the system where you installed Atomic Red Team (Local), or on a remote machine through a PowerShell Remoting session (Remote). These instructions show you how to execute tests on the Local machine. For instruction on executing tests on a Remote Machine see [here](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execute-Atomic-Tests-(Remote)).

#### Getting Started

Before executing an atomic test you should have done the following:
* [Installed Invoke-AtomicRedTeam](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Installing-Invoke-AtomicRedTeam)
* [Imported the Module](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Import-the-Module)
* [Checked Prerequisites](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Check-or-Get-Prerequisites-for-Atomic-Tests)

You may find it useful to [List Atomic Tests](https://github.com/redcanaryco/invoke-atomicredteam/wiki/List-Atomic-Tests) before execution as well.

#### Execute Specific Attacks (by Atomic Test Number) for a Given Technique

```powershell
Invoke-AtomicTest T1218.010 -TestNumbers 1,2
# or using the short form ..
Invoke-AtomicTest T1218.010-1,2
```

This assumes your atomics folder is in the default location of `<BASEPATH>\AtomicRedTeam\atomics`

Where `<BASEPATH>` is `C:` in Windows or `~` in Linux/MacOS

You can override the default path to the atomics folder using the `$PSDefaultParameterValues` preference variable as shown below.

```powershell
$PSDefaultParameterValues = @{"Invoke-AtomicTest:PathToAtomicsFolder"="C:\Users\myuser\Documents\code\atomic-red-team\atomics"}
```

Tip: Add this to your PowerShell profile so it is always set to your preferred default value.

#### Execute Specific Attacks (by Atomic Test Name) for a Given Technique

```powershell
Invoke-AtomicTest T1218.010 -TestNames "Regsvr32 remote COM scriptlet execution","Regsvr32 local DLL execution"
```

#### Execute Specific Attacks (by Atomic Test GUID) for a Given Technique

```powershell
Invoke-AtomicTest T1003 -TestGuids 5c2571d0-1572-416d-9676-812e64ca9f44,66fb0bc1-3c3f-47e9-a298-550ecfefacbc
```
Execution by GUID is useful when scripting because the GUID's are guaranteed to not change, whereas the test number and test name for a given test may change.

#### Execute All Attacks for a Given Technique

```powershell
Invoke-AtomicTest T1218.010
```

#### Specify a Process Timeout

```powershell
Invoke-AtomicTest T1218.010 -TimeoutSeconds 15
```

If the attack commands do not exit (return) within in the specified `-TimeoutSeconds`, the process and it's children will be forcefully terminated. The default value of `-TimeoutSeconds` is 120. This allows the `Invoke-AtomicTest` script to move on to the next test.

#### Execute Tests Interactively

You can execute tests in a way that lets you give input to the test during execution. For example, the commands executed my prompt you for confirmation before overwriting a file. In order to be able to do this you must specify the `-Interactive` flag. If you don't specify the `-Interactive` flag and a command asks for user input, the execution will hang until it eventually times out.

```powershell
Invoke-AtomicTest T1003 -Interactive
```

The drawback of using the `-Interactive` flag is that you can't redirect output from the command execution to a file.

#### Execute All Tests

This is not recommended but you can execute all Atomic tests in your atomics folder with the following:

```powershell
Invoke-AtomicTest All
```

A better way to do this would be to use a little PowerShell script to run each test one at a time, getting the prereqs first and cleaning up after each one. The following example runs all automated windows atomics.

```powershell
$techniques = gci C:\AtomicRedTeam\atomics\* -Recurse -Include T*.yaml | Get-AtomicTechnique

foreach ($technique in $techniques) {
    foreach ($atomic in $technique.atomic_tests) {
        if ($atomic.supported_platforms.contains("windows") -and ($atomic.executor -ne "manual")) {
            # Get Prereqs for test
            Invoke-AtomicTest $technique.attack_technique -TestGuids $atomic.auto_generated_guid -GetPrereqs
            # Invoke
            Invoke-AtomicTest $technique.attack_technique -TestGuids $atomic.auto_generated_guid
            # Sleep then cleanup
            Start-Sleep 3
            Invoke-AtomicTest  $technique.attack_technique -TestGuids $atomic.auto_generated_guid -Cleanup
        }
    }
}
```

#### Execute All Tests from a Specific Directory

Specify a custom path to your atomics folder, example C:\AtomicRedTeam\atomics

```powershell
Invoke-AtomicTest All -PathToAtomicsFolder C:\AtomicRedTeam\atomics
```

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