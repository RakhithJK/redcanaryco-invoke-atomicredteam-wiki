The Atomic Runner functionality allows you to run a configurable list of atomic tests unattended in a way that aids prevention and detection reporting. The scripts are designed to run all the tests you have listed in the CSV schedule once per week by default. Before it runs each atomic test it appends the atomic test GUID to the end of the computer hostname. This makes it easier to determine which detections fired from which atomics because the hostname in the detection will include the GUID of the atomic test that was running at the time. The cleanup commands are run after atomic test execution as well.

This script works for Windows, Linux and macOS. You need to have PowerShell Core installed on Linux/macOS to use these scripts

# Setup and Configuration
## Install `Atomic Red Team` and `Invoke-AtomicRedTeam`

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam -getAtomics -Force -noPayloads
```
## Set Custom Config using privateConfig.ps1

There is a config file called **config.ps1** in the **\<installFolder\>\\Invoke-AtomicRedTeam\\Public** folder. You can optionally modify any of the default values in this config file by creating a file called **privateConfig.ps1** in the **\<installFolder\>**.

| config variable | description |
|:---:|:---:|
| PathToInvokeFolder | The folder containing the installed Invoke-AtomicRedTeam folder | 
| PathToPublicAtomicsFolder | The folder containing the installed atomics folder |
| PathToPrivateAtomics | The folder containing your own private atomics (if any) |
| user | The user/account that will be used to execute atomics |
| basePath | The path where you want the folder created that houses the logs and the runner schedule. |
| scheduleTimeSpan | The time span in which you want all of the atomics on your schedule to complete. |
| kickOffDelay | A delay specified as Timespan to sleep before running the atomic |
| syslogServer | Set this to the name of your syslog server if you want to use the SysLog execution logger |
| syslogPort | The port for the syslog server (ignored if syslogServer not set) |
| LoggingModule | The logging module to use the atomic execution logs (e.g. Attire-ExecutionLogger or Syslog-ExecutionLogger |
| verbose | Set to $true for more output in the runner logs |
| logFolder | Name of the folder that will be found in the basePath and contains the Runner logs |
| CustomTag | A string that you want sent with each execution log sent to the SysLog logger  |
| absb | An optional AMSI bypass script block that will be run before each atomic (Windows Only) |

Table of default values:

| config variable | default (Windows) | default (Linux/macOS) |
|:---:|:---:|:---:|
| PathToInvokeFolder | C:\AtomicRedTeam\Invoke-AtomicRedTeam | ~/AtomicRedTeam/Invoke-AtomicRedTeam | 
| PathToPublicAtomicsFolder | C:\AtomicRedTeam\atomics | ~/AtomicRedTeam/atomics |
| PathToPrivateAtomics | C:\PrivateAtomics\atomics | ~/PrivateAtomics/atomics |
| user | $env:USERDOMAIN\\$env:USERNAME | $env:USER |
| basePath | $env:HOME | $env:USERPROFILE |
| scheduleTimeSpan | 7 days | 7 days |
| kickOffDelay| 0 minutes | 0 minutes |
| syslogServer |  |  |
| syslogPort | 514 | 514 |
| LoggingModule| Default-ExecutionLogger | Default-ExecutionLogger |
| verbose | $false | $false |
| logFolder | AtomicRunner-Logs | AtomicRunner-Logs |
| CustomTag |  |  |
| absb | $null | $null |

Example **privateConfig.ps1**

```powershell
$artConfig | Add-Member -Force -NotePropertyMembers @{
  basePath = "C:\Users\Public"
  PathToPrivateAtomics = "C:\MyPrivateAtomics\atomics" 
}
```

## Run Invoke-SetupAtomicRunner

```powershell
# Run Invoke-SetupAtomicRunner as the runner user (from admin prompt)
Invoke-SetupAtomicRunner
```
Note: On Windows, you will be prompted to enter the credentials of the user that will be running the atomics.

The setup script does the following:

1. If you configured a group managed service account (gMSA) for renaming the computer, a JEA endpoint will be created for renaming the computer using the gMSA account.
2. Creates a scheduled task called `KickOff-AtomicRunner` to keep the Runner running after a restart (or a cron job on Linux/macOS)
3. Adds an Import-Module statement to your PowerShell profile to load Invoke-AtomicRedTeam.psd1 automatically
4. Installs the Posh-SYSLOG module it is missing and you are configured to use it.
5. Creates a CSV schedule file listing all of the atomics in your atomics folders (public and private). All atomics will be **disabled** by default.
6. Runs the **get_prereqs** commands for all atomic tests that are **enabled** (active) on the schedule.

## Enable Atomic Tests on the Schedule File

Edit the schedule file to enable some atomics to run (these will all run every X minutes as calculated using the **scheduleTimeSpan** and the number of atomic tests enabled on your schedule). By default, the schedule will be located at **<user-home-dir>\AtomicRunner\AtomcRunnerSchedule.csv**

Example **AtomicRunnerSchedule.csv** files are found [here](https://github.com/clr2of8/AttackEmulationTools/tree/main/Samples/Emulations).

## Run Invoke-SetupAtomicRunner (again) 

Run Invoke-SetupAtomicRunner (again) to get any prereqs for tests that you enabled on the schedule.

## Restart the computer to kickoff the Atomic Runner process

# The KickOff-AtomicRunner Scheduled Task

The KickOff-AtomicRunner scheduled task (or cronjob on Linux/macos) calls the Invoke-AtomicRunner function after the computer restarts. It runs 1 minutes after reboot (with retries at 2,4,8,16,32 and 64 minutes if needed - Windows only). The KickOff-AtomicRunner script also handles log rotation for the Runner scripts.

# The Invoke-AtomicRunner Function

The Invoke-AtomicRunner function does the following:
1. Parses the Atomic GUID from the end of the computer hostname to know which atomic test it should run. If no GUID is found, it assumes it should run first atomic test enabled on the schedule.
2. Executes the atomic test specified by the GUID
3. Sleeps for an amount of time calculated by the **scheduleTimeSpan** and the number of atomic tests enabled on the schedule
4. Runs the cleanup commands for the atomic
5. Renames the computer to include the GUID of the next enabled test on the schedule

To do: add notes about the stop file and updating the schedule to add new atomic tests. And add a wiki page on the new syslog logger