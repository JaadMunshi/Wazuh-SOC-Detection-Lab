# Port Scan Detection Investigation — Rule 100050

## Objective

The aim was to create a custom Wazuh rule to detect possible port scanning activity against the Windows VM.

## Investigation

I ran a controlled Nmap scan from the Kali VM against the Windows VM.
Windows Firewall generated blocked connection events, including Event ID 5157.
These events were collected by the Wazuh Agent and appeared in the Wazuh Dashboard.
I investigated which existing Wazuh rule was detecting these events so I could use it as the parent for my custom rule.
The event was traced through the Wazuh rule hierarchy:
Windows Event 5157 → Rule 60001 → Rule 60104 → Custom Rule 100050
Rule 60104 was identified as the relevant rule for the Windows audit failure events.

## Detection Development

I created Rule 100050 using Rule 60104 as the matched rule.
The rule was configured to trigger after 5 matching firewall block events from the same source IP within 10 seconds.

## Testing

I tested the rule by running controlled Nmap scanning activity from Kali against Windows.
The Windows firewall events were collected by Wazuh and Rule 100050 successfully generated a Level 10 alert.

## Result

The custom port scan detection worked successfully and was mapped to MITRE ATT&CK T1046 — Network Service Scanning.
The investigation also helped me understand how Wazuh's existing rules can be traced and used to build custom detection rules.
