To uninstall Atomic Red Team you can simply delete the default installation directory which is `<BASEPATH>\AtomicRedTeam` where `<BASEPATH>` is `C:` on Windows or `~` on Linux/macOS.

And if you'd like to be very particular, you could also uninstall the `powershell-yaml` module that gets installed with the Invoke-AtomicRedTeam installation. First, close all PowerShell sessions, then run the following command from a command prompt (Windows) or terminal (macOS/Linux).

```powershell
powershell -NoProfile -Command "Uninstall-Module powershell-yaml" # this is for Windows
pwsh -NoProfile -Command "Uninstall-Module powershell-yaml" # this is for macOS/Linux
```