## Import the Module

In order make the `Invoke-AtomicTest` function available for use in your current PowerShell session you must import the module. This is done automatically for you in the PowerShell window where you installed the execution framework, but in the event that you start a new PowerShell window, you will need to re-import the module which can be done as follows.

```powershell
Import-Module "C:\AtomicRedTeam\invoke-atomicredteam\Invoke-AtomicRedTeam.psd1" -Force
```

Note: If you installed AtomicRedTeam to a different path, you would need to adjust this command accordingly.

## Add the Import to your PowerShell Profile

If you would like to ensure that the `Invoke-AtomicTest` functionality is always available, without having to manually import the module first, you can add the import statement to your PowerShell profile. See the Microsoft documentation for [how to use PowerShell profiles](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7).

Here is an example profile to import the module at startup and to set the default `-PathToAtomicsFolder`.

```powershell
Import-Module "C:\AtomicRedTeam\invoke-atomicredteam\Invoke-AtomicRedTeam.psd1" -Force
$PSDefaultParameterValues = @{"Invoke-AtomicTest:PathToAtomicsFolder"="C:\AtomicRedTeam\atomics"}
Set-Location "C:\users\art-user"
```
