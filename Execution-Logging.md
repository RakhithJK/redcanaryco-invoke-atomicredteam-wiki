# Built-in Logging Options

1) [Default Logger (csv)](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execution-Logging#default-logger)
2) [Attire Logger](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execution-Logging#attire-logger)
3) [Syslog Logger](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execution-Logging#syslog-logger)
4) [WinEvent Logger](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Execution-Logging#winevent-logger)

# Default Logger

By default, test execution details are written to `Invoke-AtomicTest-ExecutionLog.csv` in the tmp directory ($env:TEMP, %tmp%, or \tmp). Use the `-ExecutionLogPath` parameter to write to a different file. Execution is only logged when the attack commands are run (not when the `-ShowDetails` , `-CheckPrereqs`, `GetPrereqs`, or `-Cleanup` switches are used). Use the `-NoExecutionLog` switch to not write execution details to disk. 

#### Specify an Alternate Path\Filename for Writing the Execution Log

```powershell
Invoke-AtomicTest T1218.010 -ExecutionLogPath 'C:\Temp\mylog.csv'
```

The execution log records test name and number, execution time, user, and hostname. It does not include the output seen on the screen when you run the test. The following PowerShell command provides a convenient view of the execution log.

```powershell
Import-Csv $env:TEMP\Invoke-AtomicTest-ExecutionLog.csv | Out-GridView # Windows
```

```powershell
Import-Csv /tmp/Invoke-AtomicTest-ExecutionLog.csv | FT # Linux
```

#### Execution Log Example

| Execution Time (UTC) | Execution Time (Local) | Technique | Test Number | Test Name | Hostname | IP Address | Username | GUID | ProcessId | ExitCode |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 2023-06-16T14:09:24Z | 2023-06-16T08:09:24Z | T1016 | 1 | System Network Configuration Discovery on Windows | art-vm2 | 192.168.8.165 | testdomain\art | 970ab6a1-0157-4f3f-9a73-ec4166754b23 | 12584 | 0 |
| 2023-06-16T14:09:25Z | 2023-06-16T08:09:25Z | T1016 | 2 | List Windows Firewall Rules | art-vm2 | 192.168.8.165 | testdomain\art | 038263cb-00f4-4b0a-98ae-0696c67e1752 | 11796 | 0 |
| 2023-06-16T14:10:09Z | 2023-06-16T08:10:09Z | T1016 | 5 | Adfind - Enumerate Active Directory Subnet Objects | art-vm2 | 192.168.8.165 | testdomain\art | 9bb45dd7-c466-4f93-83a1-be30e56033ee | 12908 | -1 |
| 2023-06-16T14:10:10Z | 2023-06-16T08:10:10Z | T1016 | 6 | Qakbot Recon | art-vm2 | 192.168.8.165 | testdomain\art | 121de5c6-5818-4868-b8a7-8fd07c455c1b | 2160 | 0 |

#### Redirect Output From Test Execution to a File

The Attire Logger is the only logging mechanism that produces a log containing the full command input and output details. If you want to capture the command output while using one of the other loggers you can use a command like the following.

```powershell
Invoke-AtomicTest T1027 -TestNumbers 2 *>&1 | Tee-Object atomic-out.txt -Append
```

The command above will log all three output streams, everything you see on the screen, to a file called `atomic-out.txt` and the `-Append` flag will cause it to append the data to the file instead of overwrite it.

If you would like to write the errors out to a separate file so they are easier to spot you can use the following command.

```powershell
Invoke-AtomicTest T1027  -TestNumbers 2 2>>atomic-error.txt | Tee-Object atomic-out.txt -Append
```

# Attire Logger

Instead of using the default logging mechanism you can log execution details (**including command output**) in the Attire format. The Attire format is written as JSON data and is importable into tools like [Vectr](https://vectr.io/), a purple team reporting tool. The default log name when using the Attire Logger is `Invoke-AtomicTest-ExecutionLog-timestamp.json` in the tmp directory ($env:TEMP, %tmp%, or \tmp). Note that text **timestamp** in the log file name will be replaced by the actual timestamp at execution. This means that a new log file will be created every time Invoke-AtomicTest is called (because the timestamp will be different).

You can specify the use of the Attire logger when executing atomic tests as follows.

```powershell
Invoke-AtomicTest T1016 -LoggingModule "Attire-ExecutionLogger" -ExecutionLogPath T1016-Windows.json
```

This will create a JSON execution log called `T1106-Windows.json` in the current directory. You can then import this file into Vectr.

Note: The default logger appends logs to a single file, but the Attire logger overwrites the log each time Invoke-AtomicTest is called.

If you'd like to keep all your logs without having to specify a new name every time, you use the **timestamp** place holder in the name which will be replaced by the actual execution timestamp at run time. The following command would write a new log every time using the current time stamp as the name of the file.

```powershell
Invoke-AtomicTest T1016 -LoggingModule "Attire-ExecutionLogger" -ExecutionLogPath "timestamp.json")
```

Lastly, if you don't want to specify the `LoggingModule` manually every time, you can add the following line to your PowerShell profile.

```powershell
$PSDefaultParameterValues = @{"Invoke-AtomicTest:LoggingModule"="Attire-ExecutionLogger"}
```

Alternatively, you could set the logger option via your [privateConfig.ps1 file](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Continuous-Atomic-Testing#set-custom-config-using-privateconfigps1).

Click [here](https://www.youtube.com/watch?v=n-C9ovMFYnk) for a demo of importing the Attire logs into [Vectr](https://vectr.io/).

Looking for a way to merge multiple Attire logs into one file? Look [here](https://github.com/Retrospected/attire-merger)

# Syslog Logger

Instead of using the default logging mechanism, you can log execution details directly to a Syslog server. Use the **privateConfig.ps1** file described [here](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Continuous-Atomic-Testing) to specify your Syslog server and port. You can set the **LoggingModule** to **Syslog-ExecutionLogger** or just leave it blank.

```powershell
Invoke-AtomicTest T1016 -LoggingModule "Syslog-ExecutionLogger"
```

The SysLog messages will contain a JSON formatted string with the following information:

* Execution Time (UTC)
* Execution Time (Local)
* Technique
* Test Number
* Test Name
* Hostname
* IP Address
* Username
* GUID
* Tag
* CustomTag

Where the **Tag** is set to **atomicrunner** and **CustomTag** is configurable via your **privateConfig.ps1** file.

# WinEvent Logger

You can log execution details directly to the Windows event log as shown below. You will find the execution details in the `Atomic Red Team` log under the `Application and Services Logs` folder in the Windows event viewer.

```powershell
Invoke-AtomicTest T1016 -LoggingModule "WinEvent-ExecutionLogger"
```

Note: The very first time you use the WinEvent logger you will need to do so as admin so that the `Atomic Red Team` log can be created. After it has been created you no longer need to invoke tests as an admin user to get the execution details logged.