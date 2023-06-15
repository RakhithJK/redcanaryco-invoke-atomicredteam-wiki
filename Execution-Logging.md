# Default Logger

By default, test execution details are written to `Invoke-AtomicTest-ExecutionLog.csv` in the tmp directory ($env:TEMP, %tmp%, or \tmp). Use the `-ExecutionLogPath` parameter to write to a different file. Execution is only logged when the attack commands are run (not when the `-ShowDetails` , `-CheckPrereqs`, `GetPrereqs`, or `-Cleanup` switches are used). Use the `-NoExecutionLog` switch to not write execution details to disk. 

#### Specify an Alternate Path\Filename for Writing the Execution Log

```powershell
Invoke-AtomicTest T1218.010 -ExecutionLogPath 'C:\Temp\mylog.csv'
```

The execution log records test name and number, execution time, user, and hostname. It does not include the output seen on the screen when you run the test. The following PowerShell command provides a convenient view of the execution log.

```powershell
Import-Csv $env:TEMP\Invoke-AtomicTest-ExecutionLog.csv | Out-GridView
```

#### Redirect Output From Test Execution to a File

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

If you'd like to keep all your logs without having to specify a new name every time, you could auto generate the name. The following command would write a new log every time using the current time stamp as the name of the file.

```powershell
Invoke-AtomicTest T1016 -LoggingModule "Attire-ExecutionLogger" -ExecutionLogPath ((Get-Date -UFormat %s) + ".json")
```

Lastly, if you don't want to specify the `LoggingModule` manually every time, you can add the following line to your PowerShell profile.

```powershell
$PSDefaultParameterValues = @{"Invoke-AtomicTest:LoggingModule"="Attire-ExecutionLogger"}
```

Alternatively, you could set the logger option via your [privateConfig.ps1 file](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Continuous-Atomic-Testing#set-custom-config-using-privateconfigps1).

Click [here](https://www.youtube.com/watch?v=n-C9ovMFYnk) for a demo of importing the Attire logs into [Vectr](https://vectr.io/).

# Syslog Logger

Instead of using the default logging mechanism you can log execution details directly to a Syslog server. Use the **privateConfig.ps1** file described [here](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Continuous-Atomic-Testing) to specify your Syslog server and port. You can set the **LoggingModule** to **Syslog-ExecutionLogger** or just leave it blank.

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
* Username
* GUID
* Tag
* CustomTag

Where the **Tag** is set to **atomicrunner** and **CustomTag** is configurable via your **privateConfig.ps1** file.