These examples assume you have already [installed the Invoke-AtomicRedTeam execution framework](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Installing-Invoke-AtomicRedTeam).

## Count the Number of Atomic Tests per Platform

```Powershell
$path = "C:\AtomicRedTeam\atomics\*"  # set this to point to your atomics folder
$techniques = gci $path -Recurse -Include T*.yaml | Get-AtomicTechnique

$windows = $linux = $macos = $cloud = 0
foreach ($technique in $techniques) {
    foreach ($atomic in $technique.atomic_tests) {
        if ($atomic.supported_platforms.contains("windows")) {
            $windows = $windows + 1
        }
        if ($atomic.supported_platforms.contains("linux")) {
            $linux = $linux + 1
        }
        if ($atomic.supported_platforms.contains("macos")) {
            $macos = $macos + 1
        }
        if (-not ($atomic.supported_platforms.contains("windows") -or $atomic.supported_platforms.contains("linux") -or $atomic.supported_platforms.contains("macos"))) {
            $cloud = $cloud + 1
        }
    }
}
Write-Host -ForegroundColor Cyan "Windows Tests: $windows"
Write-Host -ForegroundColor Green "  Linux Tests: $linux"
Write-Host -ForegroundColor Yellow "  macOS Tests: $macos"
Write-Host -ForegroundColor Magenta "  Cloud/Container Tests: $cloud"
```

## Print the GUID, Input Args, and Commands for the macOS platform

```PowerShell
$path = "C:\AtomicRedTeam\atomics\*"  # Set this to point to your atomics folder
$techniques = Get-ChildItem $path -Recurse -Include T*.yaml | Get-AtomicTechnique

foreach ($technique in $techniques) {
    foreach ($atomic in $technique.atomic_tests) {
        if ($atomic.supported_platforms.contains("macos")) {
            Write-Host -Fore Green $atomic.auto_generated_guid + "`n"
            foreach ($inputArg in $atomic.input_arguments.keys) {
                Write-Host -Fore Yellow "** $inputArg **"
                Write-Host -Fore Yellow $($atomic.input_arguments[$inputArg] | Out-String)
            }
            Write-Host -Fore Cyan $atomic.executor.command
        }
    }
}
```

## Write all Atomic Tests that require elevation to a CSV File

```PowerShell
$path = "C:\AtomicRedTeam\atomics\*"  # Set this to point to your atomics folder
$techniques = Get-ChildItem $path -Recurse -Include T*.yaml | Get-AtomicTechnique
$csvFile =  "AtomicsRequiringAdmin.csv"
remove-item $csvFile -Force -ErrorAction Ignore
foreach ($technique in $techniques) {
    foreach ($atomic in $technique.atomic_tests) {
        if ($atomic.executor.elevation_required ) {
            $details = [PSCustomObject][ordered]@{ 
                "Technique"              = $technique.attack_technique[0]
                "Test Name"              = $atomic.name
                "GUID"                   = $atomic.auto_generated_guid
            } 
            $details | Export-Csv -Path $csvFile -NoTypeInformation -Append
            Write-Host -Fore Green $details
        }
    }
}
```