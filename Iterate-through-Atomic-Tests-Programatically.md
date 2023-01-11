## Count the Number of Atomic Tests per Platform

```Powershell
$path = C:\AtomicRedTeam\atomics\*  # set this to point to your atomics folder
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