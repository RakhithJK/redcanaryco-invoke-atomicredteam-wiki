Adversary Emulation is possible by executing atomic tests in sequence. The Atomic Runner functionality allows you to run a configurable list of atomic tests base on a CSV list of atomic tests. The CSV file specifies each atomic tests to be run and can optionally specify custom input arguments and execution timeout values.

This script works for Windows, Linux and MacOS. You will need to have PowerShell Core installed on Linux/MacOS to use these scripts.

# Execute Atomics Tests from the CSV Schedule

Pass a CSV schedule to **Invoke-AtomicRunner**, optionally getting prereqs for each atomic test first and then cleanup up after.

```powershell
# This will list the name of each enabled test on the schedule (IcedID.csv in the current directory)
Invoke-AtomicRunner -listOfAtomics .\IcedID.csv -ShowDetailsBrief
```

```powershell
# This will run the get-prereq commands for each enabled test on the schedule
Invoke-AtomicRunner -listOfAtomics .\IcedID.csv -GetPrereqs
```

```powershell
# This will run each of the atomic tests on the schedule
Invoke-AtomicRunner -listOfAtomics .\IcedID.csv
```

```powershell
# This will run each the cleanup commands for each of the atomic tests on the schedule
Invoke-AtomicRunner -listOfAtomics .\IcedID.csv -Cleanup
```

# Setup and Configuration
## Install Atomic Red Team and Invoke-AtomicRedTeam

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam -getAtomics -Force -noPayloads
```
## Create the CSV Schedule

To automatically create a CSV file of the right format, containing all the atomic tests in your atomics folder, use the **Invoke-GenerateNewSchedule** command.

```powershell
Invoke-GenerateNewSchedule
```
Edit the schedule file to enable some atomics to run. By default, the schedule will be located at **<user-home-dir>\AtomicRunner\AtomcRunnerSchedule.csv**

Example **AtomicRunnerSchedule.csv** files are found [here](https://github.com/clr2of8/AttackEmulationTools/tree/main/Samples/Emulations).

* The Order column is only used for your convenience if you want to sort the schedule in Excel. It is not used programmatically.
* You can set a custom timeout value with the TimeoutSeconds column.
* The examples show how to pass custom InputArgs to the atomic.
* Notes are only for your reference and not used programmatically.

## Refresh the Schedule

As new atomics are added, removed, or edited in the Atomic Red team library, you may want to refresh your schedule. Refreshing the schedule will update the Atomic test name, technique and supported platforms for each test on the schedule, remove entries for atomic tests that have been removed, and add any new tests that aren't already on your schedule. Any new tests added to the schedule will be disabled by default, change the **enabled** value to True if you want it to be active.

```powershell
# This will refresh the CSV schedule found in the user's home directory at AtomicRunner\AtomicRunnerSchedule.csv
Invoke-RefreshSchedule
```

# Runner Logs

Additional logs are added to the **AtomicRunner-logs** folder in the home directory of the current user.
