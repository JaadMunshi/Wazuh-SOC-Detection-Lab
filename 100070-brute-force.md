Brute-Force Detection — Rule 100070



## Overview



A custom Wazuh correlation rule was created to detect repeated Windows logon failures against the same account within a short period.



## Detection Logic



- Rule ID: 100070

- Parent Rule: 60122

- Threshold: 5 events within 60 seconds

- Correlation: Same username

- Alert Level: 10

- MITRE ATT&CK: T1110 — Brute Force



## Wazuh Rule



```xml

<group name="custom_authentication_detection,">
  <rule id="100070" level="10" frequency="5" timeframe="60">
    <if_matched_sid>60122</if_matched_sid>
    <same_field>win.eventdata.targetUserName</same_field>
    <description>Possible brute-force attack against account: repeated Windows logon failures.</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>
</group>
```
## Testing

The detection was tested by generating five failed Windows logon attempts against the same account within 60 seconds.

The failed logons were collected by the Wazuh Agent and processed by the Wazuh Manager.

The rule successfully generated a Level 10 alert after the threshold was reached.

## Result

The detection successfully identified repeated failed logon attempts against the same account and mapped the activity to MITRE ATT&CK T1110.


