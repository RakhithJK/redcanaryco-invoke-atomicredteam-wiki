By default, test execution details are written to `Invoke-AtomicTest-ExecutionLog.csv` in the tmp directory ($env:TEMP, %tmp%, or \tmp). Use the `-ExecutionLogPath` parameter to write to a different file. Execution is only logged when the attack commands are run (not when the `-ShowDetails` , `-CheckPrereqs`, `GetPrereqs`, or `-Cleanup` switches are used). Use the `-NoExecutionLog` switch to not write execution details to disk. 

#### Specify an Alternate Path\Filename for Writing the Execution Log

```powershell
Invoke-AtomicTest T1218.010 -ExecutionLogPath 'C:\Temp\mylog.csv'
```

#### Redirect all Output From Test Execution to a File

```powershell
Invoke-AtomicTest tT1027 -TestNumbers 2 *>&1 | Tee-Object atomic-out.txt -Append
```