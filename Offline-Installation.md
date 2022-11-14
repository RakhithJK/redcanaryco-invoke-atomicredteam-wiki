To prepare for offline installation you should install Atomic Red Team on an online system of the same OS where the offline install is to be done. This allows us to easily grab all of the needed files from the online system and move them to the offline system. The instructions below are specific to Windows but you can adjust to make it work on Linux/macOS as well.

## Steps to take from the **online** system:

1) Install Atomic Red Team on the **online** system as shown here.
2) Get the prereqs for all tests so we can copy as many as possible to the **offline** system. Use `Invoke-AtomicTest -All -GetPrereqs` (preferably with AV disabled). You can skip\cancel any of the application installs because those won't copy over to the **offline** vm.
3) Copy the following 3 dirctories to the offline system:
* `C:\AtomicRedTeam` folder
* PowerShell `Modules\powershell-yaml` folder
* The files in the `temp` directory (where several of the downloaded prereqs will be found) 
The `powershell-yaml` folder will be found at `$HOME\Documents\PowerShell\Modules` or `$env:ProgramFiles\PowerShell\Modules`. Make sure the file paths and folders are the same on the **offline** system as the **online**.

## Steps to take from the **offline** system: