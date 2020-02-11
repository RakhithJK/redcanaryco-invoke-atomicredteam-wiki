This execution framework (Invoke-AtomicRedTeam) works cross-platform on Windows, Linux and MacOS. However, to use it on Linux and Mac you must install PowerShell Core. See [Installing PowerShell Core on Linux](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-6) and [Installing PowerShell Core on MacOS](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6) for details.

## Install Execution Framework Only

To install the execution framework (Invoke-AtomicRedTeam) run the following command from a PowerShell prompt:

`IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/install-atomicredteam.ps1'); Install-AtomicRedTeam`

If you get an Import-Module error stating that the module "cannot be loaded because running scripts is disabled on this system", restart powershell using `powershell -exec bypass` or bypass execution policy with one of [these](https://blog.netspi.com/15-ways-to-bypass-the-powershell-execution-policy/) methods and try again. Method 12 is especially promising.

By default, the installer will download and install the execution framework to `<BASEPATH>\AtomicRedTeam`

Where `<BASEPATH>` is `C:` in Windows or `~` in Linux/MacOS

Installing the execution framework (Invoke-AtomicRedTeam) does not download the repository of atomic test definitions by default (aka the [Atomics Folder](https://github.com/redcanaryco/atomic-red-team/tree/master/atomics)). This is because the atomics folder contains many files likely to trigger AV alerts on the endpoint. You may choose to white-list the install directory (`<BASEPATH>\AtomicRedTeam` by default) so that files are not quarantined or removed. Or you may choose to copy a version of the atomics folder over to the system that contains only the tests you intend to run.

## Install Execution Framework and Atomics Folder

The [Atomics Folder](https://github.com/redcanaryco/atomic-red-team/tree/master/atomics) contains the test definitions, the commands that the execution framework will execute. If you would like to install the atomics folder at the same time that you install the execution framework, you can do this by adding the `-getAtomics` switch during the install of the execution framework.

`IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/install-atomicredteam.ps1'); Install-AtomicRedTeam -getAtomics`

If the execution framework or the atomics folder are already found on disk you must use the `-Force` parameter during install as follows to erase and replace these folders.

`IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/install-atomicredteam.ps1'); Install-AtomicRedTeam -getAtomics -Force`

## Install Atomics Folder Only

If you would like to install the atomics folder as a separate step or at a later time, you can do it with the `Install-AtomicsFolder` function as follows.

`IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/install-atomicredteam.ps1'); Install-AtomicsFolder`

## Optional Installation Parameters

Both the Install-AtomicRedTeam and the Install-AtomicsFolder functions have the following optional parameters:

InstallPath
- Where to install (default: C:\AtomicRedTeam on Windows or ~\AtomicRedteam on MacOS and Linux)

    `Install-AtomicRedTeam -InstallPath c:\tools\`

Force
- Remove the previous installation before installing

	`Install-AtomicRedTeam -Force`

RepoOwner
- Installfrom another GitHub repo. Default RepoOwner is "redcanaryco"

	`Install-AtomicRedTeam -RepoOwner clr2of8`

Branch
- Install from another branch. Default Branch is "master"

	`Install-AtomicRedTeam -RepoOwner clr2of8 -Branch start-process-branch`
