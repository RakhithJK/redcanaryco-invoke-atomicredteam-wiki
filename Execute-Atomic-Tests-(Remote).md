# This documentation doesn't Apply until [PR #10](https://github.com/redcanaryco/invoke-atomicredteam/pull/10) is merged into the master branch.

You can use the `Invoke-AtomicTest` function to run an atomic test on the system where you installed Atomic Red Team (Local), or on a remote machine through a PowerShell Remoting session (Remote). These instructions show you how to execute tests on the Local machine. For instruction on executing tests on the Local Machine (where Invoke-AtomicTest is installed) see [here](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execute-Atomic-Tests-(Local)).

## Prerequisites

To execute an atomic test on a remote machine, from a local machine where Invoke-AtomicTest is installed, the following prerequisites must be met.

![image](https://user-images.githubusercontent.com/22311332/76806831-23bd5a00-67a8-11ea-8feb-09faf0e6f96a.png)

#### Enable PS Remoting

If your Local and Remote machines are both Windows OS, you simply need to enable PS Remoting on the Remote machine.
Instructions to enable PS Remoting can be found [here](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-7).

**Note:** see the `-SkipNetworkProfileCheck` option if your client is using a public network profile.

By default, only users in the Adminstrators group on the Remote machine can make a PS Remoting connection.

#### Install PowerShell Core

PowerShell version 6 or higher (Core) must be installed when the Local and Remote computers are not Windows.

Click [here](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7) for instructions on installing PS Core. 

#### PowerShell Remoting over SSH

The Remote machine must be configured for PowerShell Remoting over SSH when the Local and Remote computers are not Windows.

See [this link](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core?view=powershell-7) for instructions on configuring PowerShell Remoting over SSH.

