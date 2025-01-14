This execution framework (Invoke-AtomicRedTeam) works cross-platform on Windows, Linux and MacOS. However, to use it on Linux and Mac you must install PowerShell Core. See [Installing PowerShell Core on Linux](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-6) and [Installing PowerShell Core on MacOS](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6) for details.

An alternative to installing Atomic Red Team on your device is to use it inside of a Docker container or the Windows Sandbox where it is already pre-installed.

* [Docker container with Atomic Red Team Installed](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Docker-Containers)
* [Windows Sandbox with Atomic Red Team Installed](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Windows-Sandbox)

**Minimum supported PowerShell version is 5.0**

## Install Execution Framework Only

The Invoke-AtomicRedTeam Execution is available for install from the PowerShell Gallery and can be installed with one simple command executed from a PowerShell prompt:

```powershell
Install-Module -Name invoke-atomicredteam,powershell-yaml -Scope CurrentUser
```

Note: If you get an error relating to a missing PSRepository, use `Register-PSRepository -Default` to register the needed repository.

To install the execution framework without downloading it from the PowerShell Gallery as shown above, you can continue with the following instructions:

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam
```

If you get an `Import-Module` error stating that the module "cannot be loaded because running scripts is disabled on this system", restart powershell using `powershell -exec bypass` or bypass execution policy with one of [these](https://blog.netspi.com/15-ways-to-bypass-the-powershell-execution-policy/) methods and try again. Method 12 is especially promising.

If you use the `Install-Module` method, the module will be located in your default PowerShell modules folder and you won't need to manually import the module each time you start a new PowerShell Windows. Otherwise, if you install using `Install-AtomicRedTeam`, the installer will download and install the execution framework to `<BASEPATH>\AtomicRedTeam`

Where `<BASEPATH>` is `C:` in Windows or `~` in Linux/MacOS

Installing the execution framework (Invoke-AtomicRedTeam) does not download the repository of atomic test definitions by default (aka the [Atomics Folder](https://github.com/redcanaryco/atomic-red-team/tree/master/atomics)). This is because the atomics folder contains many files likely to trigger AV alerts on the endpoint. You may choose to white-list the install directory (`<BASEPATH>\AtomicRedTeam` by default) so that files are not quarantined or removed. Or you may choose to copy a version of the atomics folder over to the system that contains only the tests you intend to run.

If you get an error of "Could no create SSL/TLS secure channel." run the following PowerShell command before your run the install commands.

```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
```
## Install Execution Framework and Atomics Folder

The [Atomics Folder](https://github.com/redcanaryco/atomic-red-team/tree/master/atomics) contains the test definitions; the commands that the execution framework will execute. If you would like to install the atomics folder at the same time that you install the execution framework, you can do this by adding the `-getAtomics` switch during the install of the execution framework.

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam -getAtomics
```

If the execution framework or the atomics folder are already found on disk you must use the `-Force` parameter during install as follows to erase and replace these folders.

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam -getAtomics -Force
```

If you prefer to download the atomics folder with only the test definition yaml files and none of the payloads from the `/src` or `/bin` directories, use the `-noPayloads` flag as follows. You can use the `-getPrereq` flag with `Invoke-AtomicTest` to download the payloads for the atomics you choose to run.

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam -getAtomics -Force -noPayloads
```

## Install Atomics Folder Only

If you would like to install the atomics folder as a separate step or at a later time, you can do it with the `Install-AtomicsFolder` function as follows.

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicsfolder.ps1' -UseBasicParsing);
Install-AtomicsFolder
```


If you prefer to download the atomics folder with only the test definition yaml files and none of the payloads from the `/src` or `/bin` directories, use the `-noPayloads` flag as follows. You can use the `-getPrereq` flag with `Invoke-AtomicTest` to download the payloads for the atomics you choose to run.

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicsfolder.ps1' -UseBasicParsing);
Install-AtomicsFolder -noPayloads -Force
```

## Optional Installation Parameters

Both the Install-AtomicRedTeam and the Install-AtomicsFolder functions have the following optional parameters:

InstallPath
- Where to install (default: C:\AtomicRedTeam on Windows or ~\AtomicRedteam on MacOS and Linux)

```powershell
Install-AtomicRedTeam -InstallPath "c:\tools"
Install-AtomicsFolder -InstallPath "c:\tools"
```

Force
- Remove the previous installation before installing

```powershell
Install-AtomicRedTeam -Force
Install-AtomicsFolder -Force
```

RepoOwner
- Install from another GitHub repo. Default RepoOwner is "redcanaryco"

```powershell
Install-AtomicRedTeam -RepoOwner "clr2of8"
Install-AtomicsFolder -RepoOwner "clr2of8"
```

Branch
- Install from another branch. Default Branch is "master"

```powershell
Install-AtomicRedTeam -RepoOwner "clr2of8" -Branch "start-process-branch"
Install-AtomicsFolder -RepoOwner "clr2of8" -Branch "start-process-branch"
```

## Offline Installation

[Offline Installation Instructions](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Offline-Installation)