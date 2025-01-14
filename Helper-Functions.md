The following auxiliary functions are helpful to simplify atomic test creation:

1) [`New-Atomic*`](https://github.com/redcanaryco/invoke-atomicredteam/wiki/New-Atomic*-Technique-Test-Creation-Functions) - A set of functions to facilitate using native PowerShell to create and validate atomic techniques and tests.
2) [`Invoke-WebRequestVerifyHash`](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Invoke-WebRequestVerifyHash) - Useful as part of a GetPrereq Command to validate a Prereq file before download.

You can iterate through atomic tests programmatically with the [`Get-AtomicTechnique`](https://github.com/redcanaryco/invoke-atomicredteam/blob/master/Public/Get-AtomicTechnique.ps1) function as shown in [these examples](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Iterate-through-Atomic-Tests-Programmatically).