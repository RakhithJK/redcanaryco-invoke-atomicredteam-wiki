Each atomic test definition defines whether the test is intended to be run on Windows, Linux or MacOS. However, there may be finer grained requirements for an atomic. For example, the atomic test may be intended for execution on a Domain Controller or Server rather than a Workstation. Other requirements (aka Prerequisites) may be that certain files or users exists and specific tools are installed.

# Check Prerequisites

To check if the system you are using meets the prerequisites required for each test, use the `-CheckPrereqs` switch before executing the test.

```Invoke-AtomicTest T1003 -TestName "Windows Credential Editor" -CheckPrereqs```

The command above checks that the system meets the prerequisites for running the "Windows Credential Editor" test under Technique 1003. If the output includes **[*] Elevation required but not provided**, you need to run the command from an elevated command prompt.

To check the prerequisites for all atomic tests within a given technique number you can use the following command.

```Invoke-AtomicTest T1003 -CheckPrereqs```

# Get Prerequisites

If you find that your system does not meet the prerequisites, you can use the `-GetPrereqs` switch to attempt to satisfy those prerequisites as follows.

```Invoke-AtomicTest T1003 -TestName "Windows Credential Editor" -GetPrereqs```

If this is successful, the output will indicate that the prerequisites are now successfully met. However, sometimes the output may indicate that the requirements will have to be met manually.