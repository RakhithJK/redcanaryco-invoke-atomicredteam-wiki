Now that the execution framework is [installed](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Installing-Atomic-Red-Team) and the module is [imported](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Import-the-Module) you are ready to start using it. A good starting point is to list the Technique numbers and test names available for execution.

## Show Details Brief

Use the `-ShowDetailsBrief` switch to list the tests available for a given technique number.

```Invoke-AtomicTest T1003 -ShowDetailsBrief```

If you would like to show details for all techniques, you can use "All" as the technique number.

```Invoke-AtomicTest All -ShowDetailsBrief```

Showing the brief details is an easy way to identify the atomic test number associated with each test, which can then be used to specify individual tests to execute.

## Show Details (verbose)

Use the `-ShowDetails` switch to show test details, including attack commands, input parameters, and prerequisites for a given technique number.

```Invoke-AtomicTest T1003 -ShowDetails```
