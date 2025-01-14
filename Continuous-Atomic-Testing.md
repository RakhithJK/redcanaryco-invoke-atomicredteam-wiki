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

| Configuration Variable | Description |
|:---:|:---:|
| PathToInvokeFolder | The folder containing the installed Invoke-AtomicRedTeam folder | 
| PathToPublicAtomicsFolder | The folder containing the installed atomics folder |
| PathToPrivateAtomics | The folder containing your own private atomics (if any) |
| user | The user/account that will be used to execute atomics |
| basePath | The path where you want the folder created that houses the logs and the runner schedule. |
| scheduleTimeSpan | The time span in which you want all of the atomics on your schedule to complete. |
| scheduleFileName | The name of the csv file containing the schedule (list) of atomic tests to run. |
| kickOffDelay | A delay (specified as a PowerShell Timespan object) to sleep before running the atomic |
| syslogServer | Set this to the name of your syslog server if you want to use the SysLog execution logger |
| syslogPort | The port for the syslog server (ignored if syslogServer not set) |
| syslogProtocol | The port for the network protocol to use with the syslog server (options are UDP, TCP, TCPwithTLS) |
| LoggingModule | The logging module to use for the atomic execution logs (e.g. Attire-ExecutionLogger, Syslog-ExecutionLogger or WinEvent-ExecutionLogger |
| logFileName| The file name to use for the atomic execution logs, including the file extension (log will be written to the folder specified by `logFolder` |
| verbose | Set to $true for more output in the runner logs |
| debug | Set to $true for additional output which will be added to a file called all-out-\<base hostname\>.txt |
| logFolder | Name of the folder that will be found in the basePath and contains the Runner logs |
| CustomTag | A string that you want sent with each execution log sent to the SysLog logger  |
| absb | An optional AMSI bypass script block that will be run before each atomic (Windows Only) |
| ServiceInstallDir | The directory to run the atomicrunnerservice executable from (Windows Only) |

Table of default values:

| config variable | default (Windows) | default (Linux/macOS) |
|:---:|:---:|:---:|
| PathToInvokeFolder | C:\AtomicRedTeam\Invoke-AtomicRedTeam | ~/AtomicRedTeam/Invoke-AtomicRedTeam | 
| PathToPublicAtomicsFolder | C:\AtomicRedTeam\atomics | ~/AtomicRedTeam/atomics |
| PathToPrivateAtomics | C:\PrivateAtomics\atomics | ~/PrivateAtomics/atomics |
| user | $env:USERDOMAIN\\$env:USERNAME | $env:USER |
| basePath | $env:HOME | $env:USERPROFILE |
| scheduleTimeSpan | 7 days | 7 days |
| scheduleFileName | AtomicRunnerSchedule.csv | AtomicRunnerSchedule.csv |
| kickOffDelay| 0 minutes | 0 minutes |
| syslogServer |  |  |
| syslogPort | 514 | 514 |
| syslogProtocol | UDP | UDP |
| LoggingModule| Default-ExecutionLogger | Default-ExecutionLogger |
| logFileName| <Local TimeStamp>_<Base Hostname)-ExecLog.csv | <Local TimeStamp>_<Base Hostname)-ExecLog.csv |
| verbose | $false | $false |
| debug| $false | $false |
| logFolder | AtomicRunner-Logs | AtomicRunner-Logs |
| CustomTag |  |  |
| absb | $null | $null |
| ServiceInstallDir | "${ENV:windir}\System32" | |

Example **privateConfig.ps1**

```powershell
$artConfig | Add-Member -Force -NotePropertyMembers @{
  PathToPrivateAtomics = "C:\MyPrivateAtomics\atomics"
  scheduleTimeSpan = New-TimeSpan -Days 1 
  verbose = $true
  LoggingModule = 'Attire-ExecutionLogger'
  logFileName = "timestamp.json"
}
```

**Note**: You must start a new PowerShell window for any changes to the **privateConfig** file to take effect.

## Run Invoke-SetupAtomicRunner

Note: On macOS and Linux, a cronjob will be used for unattended running of atomics. On Windows, a service will be used by default but you can optionally choose to set it up as a scheduled task as described below.

```powershell
# Run Invoke-SetupAtomicRunner as the runner user (from admin prompt)
# This will prompt you for the user password, which it will use to install the atomicrunnerservice
Invoke-SetupAtomicRunner

# If you want to skip the setup of the atomicrunnerservice during the setup (because you've already set it up and don't want to enter the user password again)
Invoke-SetupAtomicRunner -SkipServiceSetup

# If you want to specify an alternative directory for the service install (default is the System32 directory)
Invoke-SetupAtomicRunner -ServiceInstallDir "C:\Temp"

# If you prefer to use a Scheduled Task instead of a service for the continuous execution of atomics
Invoke-SetupAtomicRunner -asScheduledTask
```
Note: On Windows, you will be prompted to enter the credentials of the user that will be running the atomics.

The setup script does the following:

1. Creates a service called `atomicrunnerservice` (default) or a scheduled task called `KickOff-AtomicRunner` to keep the Runner running after a restart (or a cron job on Linux/macOS)
3. Adds an Import-Module statement to your PowerShell profile to load Invoke-AtomicRedTeam.psd1 automatically
4. Installs the Posh-SYSLOG module if it is missing and you are configured to use it.
5. Creates a CSV schedule file listing all of the atomics in your atomics folders (public and private). All atomics will be **disabled** by default.
6. Runs the **get_prereqs** commands for all atomic tests that are **enabled** (active) on the schedule.

Note (Windows Only): 
* If using the Windows Service - The user must have the "Log on as a service" right. To add that right, open the Local Security Policy management console, go to the `\Security Settings\Local Policies\User Rights Assignments` folder, and edit the "Log on as a service" policy there
* If using the Windows Scheduled Task -Your Local Security Policy (Security Settings -> Local Polices -> Security Option) "Network Access: Do not allow storage of passwords and credentials for network authentication" must be disabled on your Atomic Runner to allow the scheduled task to be created.

## Enable Atomic Tests on the Schedule File

Edit the schedule file to enable some atomics to run (these will all run every X minutes as calculated using the **scheduleTimeSpan** and the number of atomic tests enabled on your schedule). By default, the schedule will be located at **\<user-home-dir\>\AtomicRunner\AtomcRunnerSchedule.csv**

Example **AtomicRunnerSchedule.csv** files are found [here](https://github.com/clr2of8/AttackEmulationTools/tree/main/Samples/Emulations).

## Run Invoke-SetupAtomicRunner (again) 

Run Invoke-SetupAtomicRunner (again) to get any prereqs for tests that you enabled on the schedule.

## Restart the computer to kickoff the Atomic Runner process

# The KickOff-AtomicRunner Scheduled Task

The KickOff-AtomicRunner scheduled task (or cronjob on Linux/macOS) calls the Invoke-AtomicRunner function after the computer restarts. It runs 1 minutes after reboot (with retries at 2,4,8,16,32 and 64 minutes if needed - Windows only). The KickOff-AtomicRunner script also handles log rotation for the Runner scripts.

# The Invoke-AtomicRunner Function

The Invoke-AtomicRunner function does the following:
1. Parses the Atomic GUID from the end of the computer hostname to know which atomic test it should run. If no GUID is found, it assumes it should run the first atomic test enabled on the schedule
2. Executes the atomic test specified by the GUID
3. Sleeps for an amount of time calculated from the **scheduleTimeSpan** and the number of atomic tests enabled on the schedule
4. Runs the cleanup commands for the atomic
5. Renames the computer to include the GUID of the next enabled test on the schedule and then reboots the computer

# Runner Logs

Additional logs are added to the **AtomicRunner-logs** folder in the home directory of the current user. Set the `verbose` and `debug` variables to `$true` in your **privateConfi.ps1** file for maximum logging.

# Refresh the Schedule

As new atomics are added, removed, or edited in the Atomic Red team library, you may want to refresh your schedule. Refreshing the schedule will update the Atomic test name, technique and supported platforms for each test on the schedule, remove entries for atomic tests that have been removed, and add any new tests that aren't already on your schedule. Any new tests added to the schedule will be disabled by default, change the **enabled** value to True if you want it to be active.

```powershell
# This will refresh the CSV schedule found in the user's home directory at AtomicRunner\AtomicRunnerSchedule.csv
Invoke-RefreshExistingSchedule
```

# Stop the Runner

A simple way to stop the atomic runner from it's continuous execution of atomics and rebooting you can simply add txt file called "stop" in the **AtomicRunner** folder. You can find the **AtomicRunner** folder in the `basePath` defined above, which is the current user's profile by default. Another way is to delete the associated service, scheduled task or cron job. To temporarily stop the atomicrunnerservice on Windows, you can execute the following from an elevated command prompt.

```powershell
atomicrunnerservice -stop
```