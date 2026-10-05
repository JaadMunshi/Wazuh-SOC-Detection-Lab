# Blocked SMB Connection Investigation — Rule 100080



## Objective

My aim was to create a custom Wazuh rule to detect blocked SMB connection attempts against the Windows VM.



## Investigation

I generated a controlled TCP connection attempt from the Kali VM to port 445 on the Windows VM.

The connection was blocked by the Windows Firewall and the event was collected by the Wazuh Agent.

I investigated the event and identified Rule 60104 as the existing Wazuh rule detecting the Windows Firewall block.

I also confirmed that the destination port was 445, which is the standard port used by SMB.



## Detection Development

I created Rule 100080 using Rule 60104 as the matched rule.

The rule was configured to detect blocked connections where the destination port was 445.



## Testing

I tested the rule by generating a controlled TCP connection attempt from Kali to port 445 on the Windows VM.

The Windows Firewall blocked the connection and Rule 100080 successfully generated a Level 10 alert.



## Result

The custom SMB detection worked successfully and was mapped to MITRE ATT\&CK T1021.002 — SMB/Windows Admin Shares.

The investigation also helped me understand how Windows Firewall events can be used to detect specific network services.

