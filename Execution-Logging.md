By default, test execution details are written to `Invoke-AtomicTest-ExecutionLog.csv` in the tmp directory ($env:TEMP, %tmp%, or \tmp). Use the `-ExecutionLogPath` parameter to write to a different file. Execution is only logged when the attack commands are run (not when the `-ShowDetails` , `-CheckPrereqs`, `GetPrereqs`, or `-Cleanup` switches are used). Use the `-NoExecutionLog` switch to not write execution details to disk. 

#### Specify an Alternate Path\Filename for Writing the Execution Log

```powershell
Invoke-AtomicTest T1218.010 -ExecutionLogPath 'C:\Temp\mylog.csv'
```

The execution log records test name and number, execution time, user, and hostname. It does not include the output seen on the screen when you run the test. See the next section for details on recording test output. The following PowerShell command provides a convenient view of the execution  log.

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