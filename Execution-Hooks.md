Users can define 4 different functions that will be called (if they exist) before/after atomic execution or before/after atomic test cleanup commands:

1. Invoke-ARTPreAtomicHook
1. Invoke-ARTPostAtomicHook
1. Invoke-ARTPreAtomicCleanupHook
1. Invoke-ARTPostAtomicCleanupHook

See [here](https://github.com/clr2of8/AtomicRedTeamHooks) for an example PowerShell module where you can provide the code to run for each of these hooks.