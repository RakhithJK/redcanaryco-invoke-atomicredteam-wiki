To prepare for **offline** installation you should install Atomic Red Team on an **online** system of the same OS version where the **offline** install is to be done. This allows you to easily grab all of the needed files from the **online** system and move them to the **offline** system. The instructions below are specific to Windows but you can adjust to make it work on Linux/macOS as well.

## Steps to take from the **online** system:

1) Install Atomic Red Team on the **online** system as shown [here](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Installing-Invoke-AtomicRedTeam#install-execution-framework-and-atomics-folder).
2) Get the prereqs for all tests so you can copy as many as possible to the **offline** system. Use `Invoke-AtomicTest -All -GetPrereqs` (preferably with AV disabled). You can skip\cancel any of the application installs because those won't copy over to the **offline** system.
3) Copy the following 3 directories from the **online** system to the **offline** system:
  * `C:\AtomicRedTeam` folder
  * PowerShell `powershell-yaml` folder (`$HOME\Documents\PowerShell\Modules` or `$env:ProgramFiles\PowerShell\Modules`)
  * The files in the `temp` directory (where several of the downloaded prereqs will be found) 

Note: It is recommended that you add an AV exclusion for the `C:\AtomicRedTeam` folder so that no files from the project are quarantined or deleted.

## Steps to take from the **offline** system:

1.  Make sure the file paths of the 3 folders are the same on the **offline** system as the **online**. You should have a `c:\AtomicRedTeam` folder with two folders in it (`atomics` and `invoke-atomicredteam`). You should have a `powershell-yaml` folder at `$HOME\Documents\PowerShell\Modules` or `$env:ProgramFiles\PowerShell\Modules`
2) Import the Invoke-AtomicRedTeam module as described [here](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Import-the-Module).