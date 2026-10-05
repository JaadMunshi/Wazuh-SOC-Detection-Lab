# Port Scan Detection — Rule 100050



## Overview



A custom Wazuh correlation rule was created to detect possible port scanning activity against the Windows endpoint.



The rule looks for repeated Windows Firewall blocks from the same source IP within a short time period.



## Detection Logic



- Rule ID: 100050

- Parent Rule: 60104

- Threshold: 5 events within 10 seconds

- Correlation: Same source IP

- Alert Level: 10

- MITRE ATT&CK: T1046 — Network Service Scanning



## Wazuh Rule



```xml
</group>
<group name="custom_network_detection,">
  <rule id="100050" level="10" frequency="5" timeframe="10">
    <if_matched_sid>60104</if_matched_sid>
    <same_field>win.eventdata.sourceAddress</same_field>
    <description>Possible port scan detected: repeated Windows Firewall blocks from the same source IP.</description>
    <mitre>
      <id>T1046</id>
    </mitre>
  </rule>
</group>
```


## Testing
The detection was tested by generating controlled network scanning activity from the Kali Linux VM against the Windows VM.

Windows generated firewall block events which were collected by the Wazuh Agent and analysed by the Wazuh Manager.

The rule successfully generated a Level 10 alert after five matching events from the same source IP occurred within 10 seconds.


## Result
The detection successfully identified repeated firewall blocks consistent with possible network service scanning and mapped the activity to MITRE ATT&CK T1046.

Further investigation of the source IP and related events would be required to confirm the activity as malicious.


