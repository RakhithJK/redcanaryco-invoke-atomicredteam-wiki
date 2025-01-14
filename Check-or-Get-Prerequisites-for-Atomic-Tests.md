Each atomic test definition defines whether the test is intended to be run on Windows, Linux or MacOS. However, there may be finer grained requirements for an atomic. For example, the atomic test may be intended for execution on a Domain Controller or Server rather than a Workstation. Other requirements (aka Prerequisites) may be that certain files or users exists and specific tools are installed.

# Check Prerequisites

To check if the system you are using meets the prerequisites required for each test, use the `-CheckPrereqs` switch before executing the test.

```powershell
Invoke-AtomicTest T1003 -TestName "Windows Credential Editor" -CheckPrereqs
```

The command above checks that the system meets the prerequisites for running the "Windows Credential Editor" test under Technique 1003. If the output includes **[*] Elevation required but not provided**, you need to run the command from an elevated command prompt.

To check the prerequisites for all atomic tests within a given technique number you can use the following command.

```powershell
Invoke-AtomicTest T1003 -CheckPrereqs
```

# Get Prerequisites

If you find that your system does not meet the prerequisites, you can use the `-GetPrereqs` switch to attempt to satisfy those prerequisites as follows.

```powershell
Invoke-AtomicTest T1003 -TestName "Windows Credential Editor" -GetPrereqs
```

If this is successful, the output will indicate that the prerequisites are now successfully met. However, sometimes the output may indicate that the requirements will have to be met manually.

**Note:**  You can still execute an attack even if the pre-requisites are not met but execution may fail.

# Special Invoke-WebRequestVerifyHash Function

The execution framework provides a helpful PowerShell function called `Invoke-WebRequestVerifyHash` which only downloads and saves a file to disk if the file hash matches the specified value. This function can be used in an atomic test definition by passing in the url of the file to download, the path where it should be saved, and lastly the expected Sha256 file hash as follows:

```powershell
Invoke-WebRequestVerifyHash $url $outfile $hash
``` 

The function returns `$true` if the file was saved to disk, `$false` otherwise. See the "Windows Credential Editor" test under T1003 for example usage.

Important Note: You must add the import of `Invoke-WebRequestVerifyHash.ps1` or the entire `Invoke-AtomicRedTeam.psm1` to your PowerShell profile to make this function available to the prereq commands. See [Import the Module](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Import-the-Module) for details on this.