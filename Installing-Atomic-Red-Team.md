* Be sure to get permission and necessary approval before conducting tests. Unauthorized testing is a bad decision
and can potentially be a resume-generating event.

* Set up a test machine that would be similar to the build in your environment. Be sure you have your collection/EDR
solution in place, and that the endpoint is checking in and active. It is best to have AV turned off.

* You are responsible for understanding what a test does before executing it.

This execution framework (Invoke-AtomicRedTeam) works cross-platform on Windows, Linux and MacOS. However, to use it on Linux and Mac you must install PowerShell Core. See [Installing PowerShell Core on Linux](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-6) and [Installing PowerShell Core on MacOS](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6) for details.

To install the execution framework (Invoke-AtomicRedTeam) run the following command from a PowerShell prompt:

`IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/install-atomicredteam.ps1'); Install-AtomicRedTeam`

If you get an Import-Module error stating that the module "cannot be loaded because running scripts is disabled on this system", restart powershell using `powershell -exec bypass` or bypass execution policy with one of [these](https://blog.netspi.com/15-ways-to-bypass-the-powershell-execution-policy/) methods and try again. Method 12 is especially promising.

By default, the installer will download and Install Atomic Red Team to `<BASEPATH>\AtomicRedTeam`

Where `<BASEPATH>` is `C:` in Windows or `~` in Linux/MacOS

Running the [Install script](install-atomicredteam.ps1) locally provides three parameters:

InstallPath
- Where ART is to be Installed

    `Install-AtomicRedTeam -InstallPath c:\tools\`

DownloadPath
- Where ART is to be downloaded

    `Install-AtomicRedTeam -DownloadPath c:\tools\`

Force
- Force the new installation removing any previous installations in -InstallPath. **BE CAREFUL this will delete the entire install path folder**

	`Install-AtomicRedTeam -Force`

RepoOwner
- Install ART from another repo. Default RepoOwner is "redcanaryco"

	`Install-AtomicRedTeam -RepoOwner clr2of8`

Branch
- Install ART from another branch. Default Branch is "master"

	`Install-AtomicRedTeam -RepoOwner clr2of8 -Branch start-process-branch`

### Manual Installation


`set-executionpolicy Unrestricted`

[PowerShell-Yaml](https://github.com/cloudbase/powershell-yaml) is required to parse Atomic yaml files:

`Install-Module -Name powershell-yaml -Scope CurrentUser`

Clone the Atomic Red Team repository and import the Invoke-AtomicRedTeam module.

`import-module .\Invoke-AtomicRedTeam\Invoke-AtomicRedTeam.psm1`
