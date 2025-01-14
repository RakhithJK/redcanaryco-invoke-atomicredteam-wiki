Many atomic tests include cleanup commands to remove temporary files generated during the execution of the test or to return setting to their previous or more secure values so that the test can be run again. Running the cleanup commands after every test execution is recommended. To see what the cleanup commands do, you can use the `-ShowDetails` option as describe on the [List Atomic Tests](https://github.com/redcanaryco/invoke-atomicredteam/wiki/List-Atomic-Tests) page of this Wiki.

## Run the Cleanup Commands for the Specified Test

```powershell
Invoke-AtomicTest T1089 -TestNames "Uninstall Sysmon" -Cleanup
```

## Run the Cleanup Commands for all atomic tests under a given Technique number

```powershell
Invoke-AtomicTest T1089 -Cleanup
```