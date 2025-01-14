You can use the `Invoke-AtomicTest` function to run an atomic test on the system where you installed Atomic Red Team (Local), or on a remote machine through a PowerShell Remoting session (Remote). These instructions show you how to execute tests on a Remote machine. For instruction on executing tests on the Local Machine (where Invoke-AtomicTest is installed) see [here](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execute-Atomic-Tests-(Local)).

## Prerequisites

From a Local machine where Invoke-AtomicTest is installed, you can execute an atomic test on a Remote machine after the following prerequisites are met.

| Local Computer  | Remote Computer | Prerequisites |
| ------------- | ------------- | ------------- |
| Windows | Windows | 1) Remote must have PowerShell Remoting enabled |
| Windows | Linux, macOS | 1) Local must have PowerShell Core installed <br>2) Remote must be configured for PowerShell Remoting over SSH |
| Linux, macOS | Windows | 1) Local must have PowerShell Core installed <br>2) Remote must be configured for PowerShell Remoting over SSH |

### Enable PowerShell Remoting

If your Local and Remote machines are both Windows OS, you simply need to enable PS Remoting on the Remote machine.
Instructions to enable PS Remoting can be found [here](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/enable-psremoting).

**Note:** see the `-SkipNetworkProfileCheck` option if your client is using a public network profile.

By default, only users in the Adminstrators group on the Remote machine can make a PS Remoting connection.

### Install PowerShell Core

PowerShell version 6 or higher (Core) must be installed when the Local and Remote computers are not Windows.

Click [here](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell) for instructions on installing PS Core. 

### Configure PowerShell Remoting over SSH

The Remote machine must be configured for PowerShell Remoting over SSH when the Local and Remote computers are not Windows.

See [this link](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core) for instructions on configuring PowerShell Remoting over SSH.

Note: If your **remote** machine is Linux/macOS and you want to be able to run atomics tests that require administrative privileges you need to include `sudo` in your **sshd_config** file line as shown below AND the user you are connecting with must be able to sudo without a password.

Linux: **/etc/ssh/sshd_config**

`Subsystem powershell sudo /usr/bin/pwsh -sshs -NoLogo`

macOS: **/private/etc/ssh/sshd_config**

`Subsystem powershell sudo /usr/local/bin/pwsh -sshs -NoLogo`

## Execute Atomic Tests on Remote Machine

Before executing the test, you must first establish a PS session ($sess) to the Remote machine.

* [Establish a PS Session (From Windows to Windows)](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execute-Atomic-Tests-(Remote)#establish-a-ps-session-from-windows-to-windows)
* [Establish a PS Session (From Windows to Linux,macOS)](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execute-Atomic-Tests-(Remote)#establish-a-ps-session-establish-a-ps-session-from-linuxosx-to-windowslinuxosx)
* [Establish a PS Session (From Linux,macOS to Windows,Linux,macOS)](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execute-Atomic-Tests-(Remote)#establish-a-ps-session-establish-a-ps-session-from-linuxosx-to-windowslinuxosx)

After you have established the PS session ($sess) you can proceed with test execution as follows.

```powershell
# Install any required prerequisites on the remote machine before test execution
Invoke-AtomicTest T1218.010 -Session $sess -GetPrereqs

# execute all atomic tests in technique T1218.010 on a remote machine
Invoke-AtomicTest T1218.010 -Session $sess
```

Note that for remote execution the `PathToAtomicsFolder` always starts with $env:Temp, even if you have specified a different location on the Local Machine.

The same options that are available during Local execution are available for Remote execution as well. For example, cleanup up commands (-Cleanup), Prerequisites (-CheckPrereqs, -GetPrereqs), Show Test Details (-ShowDetails, -ShowDetailsBrief), and Prompt for custom input arguments (-PromptForInputArgs). See [Execute Atomic Tests (Local)](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execute-Atomic-Tests-(Local)) for details on each of these options.

## Establish a PS Session (From Windows to Windows)

To execute an atomic test on a remote machine, you must first [meet the prerequisites](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execute-Atomic-Tests-(Remote)#prerequisites) and establish a PowerShell Session. 

```powershell
# example session establishment from the local computer to a remote computer named 'testcomputer'
$sess = New-PSSession -ComputerName testcomputer -Credential domain\username
```

In the example above, substitute "testcomputer" with the name of the remote computer and "domain\username" with the domain (if applicable) and username of a user with administrative access on the remote machine. When you execute this command, a dialog box will appear and prompt you to enter the password for the specified user.

Now that you have a persistent session established (`$sess`), you can use it with the `Invoke-AtomicTest` function to cause execution to occur on the remote machine.

## Establish a PS Session (From Windows to Linux,macOS)

To execute an atomic test on a remote machine, you must first you must first [meet the prerequisites](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execute-Atomic-Tests-(Remote)#prerequisites) and establish a PowerShell Session. 

```powershell
# Must be using PowerShell version 6 or higher (Core) on the Local Windows Machine
# example session establishment from the local computer to a remote computer named 'testcomputer'
$sess = New-PSSession -HostName testcomputer -Username username
```

In the example above, substitute "testcomputer" with the name of the remote computer and "username" with the username of a user that can SSH to the Remote machine. When you execute this command, you will be prompted to enter the password for the specified user. You can optionally specify the -KeyFilePath option to authenticate using your private key.

Now that you have a persistent session established (`$sess`), you can use it with the `Invoke-AtomicTest` function to cause execution to occur on the remote machine.

## Establish a PS Session  (From Linux,macOS to Windows,Linux,macOS)

To execute an atomic test on a remote machine, you must first ou must first [meet the prerequisites](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execute-Atomic-Tests-(Remote)#prerequisites) and establish a PowerShell Session. 

```powershell
# example session establishment from the local computer to a remote computer named 'testcomputer'
$sess = New-PSSession -HostName testcomputer -Username username
```

In the example above, substitute "testcomputer" with the name of the remote computer and "username" with the username of a user that can SSH to the Remote machine. When you execute this command, you will be prompted to enter the password for the specified user. You can optionally specify the -KeyFilePath option to authenticate using your private key.

Now that you have a persistent session established (`$sess`), you can use it with the `Invoke-AtomicTest` function to cause execution to occur on the remote machine.