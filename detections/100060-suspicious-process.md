# Suspicious Process Detection — Rule 100060


## Overview

A custom Wazuh rule was created to detect the creation of commonly used Windows command and scripting interpreters.

The rule monitors process creation events and identifies PowerShell, Command Prompt, Windows Script Host and MSHTA.



## Detection Logic

- Rule ID: 100060

- Parent Rule: 67027

- Detection: PowerShell, CMD, WScript, CScript and MSHTA

- Alert Level: 10

- MITRE ATT&CK: T1059 — Command and Scripting Interpreter



## Wazuh Rule



```xml

<group name="custom_process_detection,">
  <rule id="100060" level="10">
    <if_sid>67027</if_sid>
    <field name="win.eventdata.newProcessName" type="pcre2">(?i)\\(powershell|cmd|wscript|cscript|mshta)\.exe$</field>
    <description>Suspicious process detected: command or scripting interpreter was created.</description>
    <mitre>
      <id>T1059</id>
    </mitre>
  </rule>
</group>
```
## Testing

Process Creation auditing was enabled on the Windows VM and controlled tests were performed using PowerShell, CMD, WScript, CScript and MSHTA.

The detection successfully generated a Level 10 alert for each tested process.

The rule uses a PCRE2 regular expression to match the executable names regardless of letter case.


## Result

The detection successfully identified the creation of commonly used Windows command and scripting interpreters and mapped the activity to MITRE ATT&CK T1059.

