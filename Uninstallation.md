To uninstall Atomic Red Team you can simply delete the default installation directory which is `<BASEPATH>\AtomicRedTeam` where `<BASEPATH>` is `C:` on Windows or `~` on Linux/MacOS.

And if you'd like to be very particular, you could also uninstall the `powershell-yaml` module that gets installed with the Invoke-AtomicRedTeam installation with the following PowerShell command.

```powershell
Uninstall-Module powershell-yaml
```