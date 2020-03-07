# Special Invoke-WebRequestVerifyHash Function

The execution framework provides a helpful PowerShell function called `Invoke-WebRequestVerifyHash` which only downloads and saves a file to disk if the file hash matches the specified value. This function can be used in an atomic test definition by passing in the url of the file to download, the path where it should be saved, and lastly the expected Sha256 file hash as follows:

```powershell
Invoke-WebRequestVerifyHash $url $outfile $hash
``` 

The function returns `$true` if the file was saved to disk, `$false` otherwise. See the "Windows Credential Editor" test under T1003 for example usage.

Important Note: You must add the import of `Invoke-WebRequestVerifyHash.ps1` or the entire `Invoke-AtomicRedTeam.psm1` to your PowerShell profile to make this function available to the prereq commands. See [Import the Module](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Import-the-Module) for details on this.