Now that the execution framework is [installed](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Installing-Atomic-Red-Team) and the module is [imported](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Import-the-Module) you are ready to start using it. A good starting point is to list the Technique numbers and test names available for execution.

## Show Details Brief

Use the `-ShowDetailsBrief` switch to list the tests available for a given technique number.

```powershell
# List atomic tests that can be run from the current platform (Windows,Linux,macOS)
Invoke-AtomicTest T1003 -ShowDetailsBrief

# List all atomic tests regardless of which platform it can be executed from
Invoke-AtomicTest T1003 -ShowDetailsBrief -anyOS
```

If you would like to show details for all techniques, you can use "All" as the technique number.

```powershell
# List atomic tests that can be run from the current platform (Windows,Linux,macOS)
Invoke-AtomicTest All -ShowDetailsBrief

# List all atomic tests regardless of which platform it can be executed from
Invoke-AtomicTest -ShowDetailsBrief -anyOS
```

Showing the brief details is an easy way to identify the atomic test number associated with each test, which can then be used to specify individual tests to execute.

## Show Details (verbose)

Use the `-ShowDetails` switch to show test details, including attack commands, input parameters, and prerequisites for a given technique number.

```powershell
# List atomic tests that can be run from the current platform (Windows,Linux,macOS)
Invoke-AtomicTest T1003 -ShowDetails

# List all atomic tests regardless of which platform it can be executed from
Invoke-AtomicTest T1003 -ShowDetails -anyOS
```
