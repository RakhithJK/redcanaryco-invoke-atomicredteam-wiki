You can use the `Invoke-AtomicTest` function to run an atomic test on the system where you installed Atomic Red Team, or on a remote machine through a [PowerShell Remoting](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/running-remote-commands?view=powershell-7#windows-powershell-remoting) session.

#### Getting Started

Before executing an atomic test you should have done the following:
* [Installed Atomic Red Team](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Installing-Atomic-Red-Team)
* [Imported the Module](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Import-the-Module)
* [Checked Prerequisites](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Check-or-Get-Prerequisites-for-Atomic-Tests)

You may find it useful to [List Atomic Tests](https://github.com/redcanaryco/invoke-atomicredteam/wiki/List-Atomic-Tests) before execution as well.

#### Execute Specific Attacks (by Atomic Test Number) for a Given Technique

```powershell
Invoke-AtomicTest T1117 -TestNumbers 1,2
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
Invoke-AtomicTest T1117 -TestNames "Regsvr32 remote COM scriptlet execution","Regsvr32 local DLL execution"
```

#### Execute All Attacks for a Given Technique

```powershell
Invoke-AtomicTest T1117
```

#### Specify a Process Timeout

```powershell
Invoke-AtomicTest T1117 -TimeoutSeconds 15
```

If the attack commands do not exit (return) within in the specified `-TimeoutSeconds`, the process and it's children will be forcefully terminated. The default value of `-TimeoutSeconds` is 120. This allows the `Invoke-AtomicTest` script to move on to the next test.

#### Execute test on a Remote Windows Machine through a PowerShell Session

To execute an atomic test on a remote Windows machine, you must first establish a PowerShell Session. Ensure that PowerShell remoting is enabled on the remote machine before you start. See the [Enabled-PSRemoting](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-7) commandlet for details.

```powershell
# example session establishment to a computer named 'testcomputer'
$sess = New-PSSession -ComputerName testcomputer -Credential domain\username
```

In the example above, substitute "testcomputer" with the name of the remote computer and "domain\username" with the domain (if applicable) and username of a user with administrative access on the remote machine. When you execute this command, a dialog box will appear and prompt you to enter the password for the specified user.

Now that you have a persistent session established (`$sess`), you can use it with the `Invoke-AtomicTest` function to cause execution to occur on the remote machine.

```powershell
# Install any required prerequisites on the remote machine before test execution
Invoke-AtomicTest T1117 -Session $sess -GetPrereqs

# execute all atomic tests in technique T1117 on a remote machine
Invoke-AtomicTest T1117 -Session $sess
```

Note that for remote execution the `PathToAtomicsFolder` always starts with $env:Temp, whereas for a local machine it typically starts with C:\AtomicRedTeam.

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

#### Specify an Alternate Path\Filename for the Execution Log

```powershell
Invoke-AtomicTest T1117 -ExecutionLogPath 'C:\Temp\mylog.csv'
```

By default, test execution details are written to `Invoke-AtomicTest-ExecutionLog.csv` in the tmp directory. Use the `-ExecutionLogPath` parameter to write to a different file. Execution is only logged when the attack commands are run (not when the `-ShowDetails` , `-CheckPrereqs`, `GetPrereqs`, or `-Cleanup` switches are used). Use the `-NoExecutionLog` switch to not write execution details to disk.

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