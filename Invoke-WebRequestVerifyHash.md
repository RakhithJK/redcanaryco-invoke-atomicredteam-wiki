# Special Invoke-WebRequestVerifyHash Function

The Invoke-WebRequestVerifyHash function only downloads and saves a file to disk if the file hash matches the specified value. This function can be used in an atomic test definition by passing in the url of the file to download, the path where it should be saved, and lastly the expected Sha256 file hash as follows:

```powershell
Invoke-WebRequestVerifyHash $url $outfile $hash
``` 

The function returns `$true` if the file was saved to disk, `$false` otherwise. See the ["Windows Credential Editor" test](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1003/T1003.md#atomic-test-3---windows-credential-editor) under T1003 for example usage.

We recommend you use the function to validate the file hash when the download location is not a Github "[Permanent Link](https://help.github.com/en/github/managing-files-in-a-repository/getting-permanent-links-to-files)", or the download location is not widely recognized as trusted. An example download source that is arguably trusted, and could forego the verify hash check, is the Microsoft SysInternals download site.
