# Blocked SMB Connection Detection — Rule 100080



## Overview



A custom Wazuh rule was created to detect blocked SMB connection attempts against the Windows endpoint.



The rule looks for Windows Firewall events where the destination port is TCP 445, the standard port used by SMB.



## Detection Logic


- Rule ID: 100080

- Parent Rule: 60104

- Detection: Blocked TCP 445 connection

- Alert Level: 10

- MITRE ATT&CK: T1021.002 — SMB/Windows Admin Shares



## Wazuh Rule



```xml

<group name="custom_smb_detection,">
  <rule id="100080" level="10">
    <if_sid>60104</if_sid>
    <field name="win.eventdata.destPort">^445$</field>
    <description>Blocked SMB connection attempt detected by Windows Firewall.</description>
    <mitre>
      <id>T1021.002</id>
    </mitre>
  </rule>
</group>
```

## Testing

The detection was tested by generating a controlled TCP connection attempt from the Kali Linux VM to port 445 on the Windows VM.

The Windows Firewall blocked the connection and the resulting event was collected by the Wazuh Agent.

The rule successfully generated a Level 10 Wazuh alert for the blocked SMB connection.


## Result

The detection successfully identified blocked SMB connection attempts against the Windows endpoint and mapped the activity to MITRE ATT&CK T1021.002.

