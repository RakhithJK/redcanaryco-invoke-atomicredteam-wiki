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
Invoke-AtomicTest tT1027 -TestNumbers 2 *>&1 | Tee-Object atomic-out.txt -Append
```

The command above will log all three output streams, everything you see on the screen, to a file called `atomic-out.txt` and the `-Append` flag will cause it to append the data to the file instead of overwrite it.

If you would like to write the errors out to a separate file so they are easier to spot you can use the following command.

```powershell
Invoke-AtomicTest T1027  -TestNumbers 2 2>>atomic-error.txt | Tee-Object atomic-out.txt -Append
```

# Attire Logger

Instead of using the default logging mechanism you can log execution details (**including command output**) in the Attire format. The Attire format is written as JSON data and is importable into tools like [Vectr](https://vectr.io/), a purple team reporting tool.

To use the Attire logging format, you must first download the `Attire-ExecutionLogger.psm1` file a import it as shown [here](https://github.com/SecurityRiskAdvisors/invoke-atomic-attire-logger).

You can specify the use of the Attire logger when executing atomic tests as follows.

```powershell
Invoke-AtomicTest T1016 -LoggingModule "Attire-ExecutionLogger" -ExecutionLogPath T1016-Windows.json
```

This will create a JSON execution log called `T1106-Windows.json` in the current directory. You can then import this file into Vectr.

Note: The default logger appends logs to a single file, but the Attire logger overwrites the log each time Invoke-AtomicTest is called.

If you'd like to keep all your logs without have to specify a new name every time, you could auto generate the name. The following command would write a new log every time using the current time stamp as the name of the file.

```powershell
Invoke-AtomicTest T1016 -LoggingModule "Attire-ExecutionLogger" -ExecutionLogPath ((Get-Date -UFormat %s) + ".json")
```

Lastly, if you don't want to specify the `LoggingModule` manually every time, you can add the following line to your PowerShell profile.

```powershell
$PSDefaultParameterValues = @{"Invoke-AtomicTest:LoggingModule"="Attire-ExecutionLogger"}
```

Helpful Hint: Add your Import-Module statements for the Invoke-AtomicRedTeam and Attire-ExecutionLogger modules to your profile as well. 