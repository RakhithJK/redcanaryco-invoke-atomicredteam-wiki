# Invoke-MalDoc
## Description
This tool was written to ease the development of MalDoc Atomics. Instead of creating a new .xlsm or .docm file for each VBA macro, this tool allows you to specify all macro functionality inside the Atomic Command without file dependencies. Invoke-Maldoc does this with powershell [Com objects](https://docs.microsoft.com/en-us/powershell/scripting/samples/creating-.net-and-com-objects--new-object-?view=powershell-7#creating-com-objects-with-new-object).

## Example Atomic.yaml

```
---
attack_technique: T1204
display_name: User Execution

atomic_tests:
- name: OSTap Style Macro Delivery
  description: |
    This Test uses a VBA macro to create and execute #{jse_path} with cscript.exe. The .jse file in turn launches wscript.exe.
    Execution is handled by [Invoke-MalDoc](https://github.com/redcanaryco/invoke-atomicredteam/blob/master/Public/Invoke-MalDoc.ps1) to load and execute VBA code into Excel or Word documents.
    
    This is a known execution chain observed by the OSTap downloader commonly used in TrickBot campaigns 
    References:
      https://www.computerweekly.com/news/252470091/TrickBot-Trojan-switches-to-stealthy-Ostap-downloader

  supported_platforms:
    - windows

  input_arguments:
    ms_office_version:
      description: |
        Microsoft Office version number found in "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office" default: 16.0
      type: String
      default: "16.0"
    ms_product:
      description: | 
        "Word" or "Excel" default: Word
      type: String
      default: Word
    jse_path:
      description: |
        Path for the macro to write out the "malicious" .jse file
      type: String
      default: 'C:\Users\Public\art.jse'

  dependency_executor_name: powershell
  dependencies:
    - description: |
        Test Requires MS Word to be installed and have been run previously. Run -GetPrereqs to run msword and build dependant registry keys
      prereq_command: |
        If (Test-Path HKCU:SOFTWARE\Microsoft\Office\#{ms_office_version}) { exit 0 } else { exit 1 }
      get_prereq_command: | 
        $msword = New-Object -ComObject word.application
        Stop-Process -Name WINWORD

  executor:
    name: powershell
    elevation_required: false
    command: |
      IEX (iwr "https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/Public/Invoke-MalDoc.ps1")
      $macrocode = "   Open `"#{jse_path}`" For Output As #1`n   Write #1, `"WScript.Quit`"`n   Close #1`n   Shell`$ `"cscript.exe #{jse_path}`"`n"
      Invoke-MalDoc $macrocode "#{ms_office_version}" "#{ms_product}"
    cleanup_command: |
      if (Test-Path #{jse_path}) { Remove-Item #{jse_path} }
      try { Remove-ItemProperty -Path 'HKCU:\Software\Microsoft\Office\#{ms_office_version}\#{ms_product}\Security\' -Name 'AccessVBOM' } catch {}
```

## Invoke-MalDoc Details

**Invoke-MalDoc** <macro_code> <office_version> <office_product>

### macro_code
1 line powershell escaped VBA subroutine code string.

**EXAMPLE** _see T1204.yaml_
```
Sub Test()
    Open "$jse_path" For Output As #1
    Write #1, "WScript.Quit"
    Close #1
    Shell$ "cscript.exe $jse_path"
End Sub
```
Becomes

<pre>"   Open `"#{jse_path}`" For Output As #1`n   Write #1, `"WScript.Quit`"`n   Close #1`n   Shell`$ `"cscript.exe #{jse_path}`"`n"</pre>

The Subroutine "Test" is directly referenced by Invoke-MalDoc and is added to the VBA code before execution.

[CyberChef Macro to escaped string converter](http://icyberchef.com/#recipe=Find_/_Replace(%7B'option':'Regex','string':'Sub.*?()%5C%5Cn'%7D,'',true,false,true)Find_/_Replace(%7B'option':'Regex','string':'End%20Sub'%7D,'',true,false,true)Find_/_Replace(%7B'option':'Simple%20string','string':'%22'%7D,'%60%22',true,false,true)Find_/_Replace(%7B'option':'Simple%20string','string':'$'%7D,'%60$',true,false,true)Find_/_Replace(%7B'option':'Extended%20(%5C%5Cn,%20%5C%5Ct,%20%5C%5Cx...)','string':'%5C%5Cn'%7D,'%60n',true,false,true)Find_/_Replace(%7B'option':'Extended%20(%5C%5Cn,%20%5C%5Ct,%20%5C%5Cx...)','string':'%5C%5Ct'%7D,'%60t',true,false,true)&input=U3ViIFRlc3QoKQogICBPcGVuICIkanNlX3BhdGgiIEZvciBPdXRwdXQgQXMgIzEKICAgV3JpdGUgIzEsICJXU2NyaXB0LlF1aXQiCiAgIENsb3NlICMxCiAgIFNoZWxsJCAiY3NjcmlwdC5leGUgJGpzZV9wYXRoIgpFbmQgU3Vi) Can be used to help!

### office_version
Numerical String of the office Version number.

This value can be found [here](https://en.wikipedia.org/wiki/Microsoft_Office#History_of_releases) or by running `Get-ChildItem Registry::HKEY_CURRENT_USER\Software\Microsoft\Office`

### office_product
This is a string to determine what office application is used. Currently supports "Excel" or "Word"