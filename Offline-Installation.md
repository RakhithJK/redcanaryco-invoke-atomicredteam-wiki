To prepare for offline installation you should install Atomic Red Team on an online system of the same OS where the offline install is to be done. This allows us to easily grab all of the needed files from the online system and move them to the offline system.

##Steps to take from the **online** system:

1) Install Atomic Red Team on the **online** system as shown here.
2) Get the prereqs for all tests so we can copy as many as possible to the **offline** system. Use `Invoke-AtomicTest -All -GetPrereqs` (preferably with AV disabled). You can skip\cancel any of the application installs because those won't copy over to the **offline** vm.
3) Create a zip file of the `C:\AtomicRedTeam` folder, the PowerShell `Modules\powershell-yaml` folder, and the `temp` directory where several of the downloaded prereqs will be found. The Modules folder will be  

##Steps to take from the **offline** system: